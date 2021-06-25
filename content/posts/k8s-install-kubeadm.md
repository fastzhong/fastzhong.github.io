+++
title = "Kubernetes 安装 - kubeadm"
date = 2020-10-04T03:08:47+08:00
readingTime = true
categories = ["Cloud Native"]
tags = ["kubernetes"]
toc = true
+++

官方推荐集群工具 kubeadm

<!--more-->

参考：
<i class="fas fa-external-link-alt"></i>&nbsp; [官方文档](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/)

单节点集群架构如下图：  
![k8s cluster](/images/k8s/single-master.jpg#center)

```md
-   K8s 集群：一个控制节点（control panel/master）；两个负载/工作节点（worker）
-   Ubuntu xenial64
-   Kubernetes 1.19.1
-   Docker 5.19
```

| 节点（角色）     | IP                     | 组件                                                                  |
| ---------------- | ---------------------- | --------------------------------------------------------------------- |
| k8s\-master1     | 192\.168\.100\.1       | kube\-apiserver，kube\-controller\-manager，kube\-scheduler，etcd     |
| ~~k8s\-master2~~ | ~~192\.168\.100\.1~~   | ~~kube\-apiserver，kube\-controller\-manager，kube\-scheduler，etcd~~ |
| k8s\-node1       | 192\.168\.100\.11      | kubelet，kube\-proxy，docker                                          |
| k8s\-node2       | 192\.168\.100\.12      | kubelet，kube\-proxy，docker                                          |
| ~~k8s\-lb~~      | ~~192\.168\.100\.100~~ | ~~Nginx, etcd~~                                                       |

## VM

VagrantFile:

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["LC_ALL"] = "en_US.UTF-8"

# ---- 虚机配置 ----
boxes = [
    {
        :name => "k8s-master1",
        :eth1 => "192.168.100.11",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "k8s-master2",
        :eth1 => "192.168.100.12",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "k8s-worker1",
        :eth1 => "192.168.100.21",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "k8s-worker2",
        :eth1 => "192.168.100.22",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "k8s-lb",
        :eth1 => "192.168.100.100",
        :mem => "1024",
        :cpu => "1"
    }
]

box = "ubuntu/xenial64"
provision_script = "yes"
synced_folder = "no"

Vagrant.configure("2") do |config|
  config.vm.box = box

# 循环设置每台虚拟机
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      # 配置 hostname
      config.vm.hostname = opts[:name]

      # 配置内存和CPU
      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
      end

      # 配置 IP
      config.vm.network :private_network, ip: opts[:eth1]

      # 设置网络代理(需要安装 vagrant-proxyconf 插件)

      # 设置启动后初始化脚本，注意 privileged: true，表示以 sudo 方式运行
      if provision_script == "yes"
        if opts[:name] != "k8s-lb"
          config.vm.provision "shell", privileged: true, path: "./setup.sh"
        end
      end

      # 设置宿主机共享文件夹(需要安装 vagrant-vbguest 插件)
      # if synced_folder == "yes"
      #   config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=774,fmode=775"]
      # end
    end
  end

  if Vagrant.has_plugin?("vagrant-cachier")
      config.cache.scope = :box
  end

  if Vagrant.has_plugin?("vagrant-hostmanager")
		config.hostmanager.enabled = true
		config.hostmanager.manage_host = true
		config.hostmanager.ignore_private_ip = false
		config.hostmanager.include_offline = true
	end

end
```

初始化脚本：

```bash
#!/bin/bash

export DEBIAN_FRONTEND=noninteractive

# 禁用防火墙启动
sudo ufw disable
sudo iptables -F -t nat && iptables -X -t nat
sudo iptables -P FORWARD ACCEPT

# 关闭 selinux
# sudo setenforce 0  # 临时
# sudo sed -i 's/enforcing/disabled/' /etc/selinux/config  # 永久

# 关闭 swap
sudo swapoff -a  # 临时
sudo sed -ri 's/.*swap.*/#&/' /etc/fstab

# 加载模块
sudo modprobe br_netfilter
# 验证模块是否生效
lsmod | grep br_netfilter

# 新建k8s.conf文件，并添加以下内容，这个是防止由于 iptables 被绕过而导致流量无法正确路由的问题。
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables=1
net.bridge.bridge-nf-call-ip6tables=1
vm.swappiness=0
EOF
sudo sysctl --system
ls /proc/sys/net/bridge

# 时间同步
sudo apt-get -y install chrony
sudo timedatectl set-timezone Asia/Singapore
sudo systemctl enable chrony
sudo systemctl start chrony

### Install packages to allow apt to use a repository over HTTPS
sudo apt-get update -y && sudo apt-get install -y apt-transport-https ca-certificates curl wget software-properties-common gnupg-agent

# 安装 docker
# Add Docker's official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
# Add the Docker apt repository:
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
stable"
# Install Docker CE
sudo apt-get update -y && sudo apt-get install -y docker-ce docker-ce-cli containerd.io
# Restart Docker
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl enable docker

# 安装 kubeadm, kubectl, kubelet (k8s二进制时则采用手动安装方式)
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update -y && sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo systemctl daemon-reload
sudo systemctl restart kubelet
sudo systemctl enable kubelet

sudo mkdir -p /opt/k8s/{bin,cfg,ssl,logs}

cat <<EOF | sudo tee /opt/k8s/env.sh
#!/bin/bash

export ETCDCTL_API=3

# kube-apiserver 的反向代理(nginx)地址端口
export KUBE_APISERVER="https://192.168.100.11:6443"

# TLS Bootstrapping 使用的 Token，可以使用 head -c 16 /dev/urandom | od -An -t x | tr -d ' ' 生成
export BOOTSTRAP_TOKEN="c47ffb939f5ca36231d9e3121a252940"

# Pod 网段
CLUSTER_CIDR="10.10.0.0/16"

# SERVICE网段
SERVICE_CIDR="10.0.0.0/24"

# Kubernetes 服务 IP (一般是 SERVICE_CIDR 中第一个IP)
CLUSTER_SERVICE_IP="10.0.0.1"

# 集群 DNS 服务 IP (从 SERVICE_CIDR 中预分配)
export CLUSTER_SERVICE_DNS_IP="10.0.0.2"

# 集群 DNS 域名（末尾不带点号）
export CLUSTER_DNS_DOMAIN="cluster.local"

EOF
sudo chmod a+x /opt/k8s/env.sh

# alias
cat <<EOF | sudo tee -a /home/vagrant/.bashrc
alias k=kubectl
alias kd="kubectl describe"
alias kx="kubectl explain"
alias ku="kubectl config use-context"
# grep: print file name, print line number, recursive, ignore-case
alias g="grep -Hnri --color"

source <(kubectl completion bash)
complete -F __start_kubectl k
export dy="--dry-run=client -o yaml"
EOF

cat > /home/vagrant/.vimrc << EOF
set tabstop=2
set expandtab
set shiftwidth=2
set nu
EOF
```

## 创建集群

kubeadm 创建 Kubernetes 的过程如下：

![kubeadm](/images/k8s/kubeadm-install.jpeg#center)

-   检查所有节点的网络配置，例如 k8s-worker1：

```bash
vagrant@k8s-worker1:~$ cat /etc/hosts
127.0.0.1	localhost

# The following lines are desirable for IPv6 capable hosts
::1	ip6-localhost	ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
ff02::3	ip6-allhosts
127.0.1.1	ubuntu-xenial	ubuntu-xenial

127.0.2.1 k8s-worker1 k8s-worker1

## vagrant-hostmanager-start
192.168.100.11	k8s-master1

192.168.100.12	k8s-master2

192.168.100.21	k8s-worker1

192.168.100.22	k8s-worker2

192.168.100.100	k8s-lb

## vagrant-hostmanager-end

vagrant@k8s-worker1:~$ ip route show
default via 10.0.2.2 dev enp0s3
10.0.2.0/24 dev enp0s3  proto kernel  scope link  src 10.0.2.15
172.17.0.0/16 dev docker0  proto kernel  scope link  src 172.17.0.1 linkdown
192.168.100.0/24 dev enp0s8  proto kernel  scope link  src 192.168.100.21
```

-   初始化集群 k8s\-master1：192\.168\.100\.11

```bash
kubeadm init \
    --pod-network-cidr=10.10.0.0/16 \
    --service-cidr=10.0.0.0/24 \
    --apiserver-advertise-address=192.168.100.11
```

配置含义：

```md
-   pod-network-cidr：pod 通信网络
-   service-cidr：service 通信网络
-   apiserver-advertise-address：kube-apiserver 端口，如果是高可用集群，要换成 LB，这里只是本机
```

成功后会看到：

```bash
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.100.11:6443 --token virvoc.4q5v8rqvuriyr3wy \
    --discovery-token-ca-cert-hash sha256:ce0f619cf549bef741ebcb8c94f0d7853ea2c11ef7573542920e18ad56f7bf40
```

依照上面的提示执行，并检查 K8s 集群的状态：

```bash
vagrant@k8s-master1:~$ mkdir -p $HOME/.kube
vagrant@k8s-master1:~$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
vagrant@k8s-master1:~$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

vagrant@k8s-master1:~$ kubectl get namespaces
NAME              STATUS   AGE
default           Active   44m
kube-node-lease   Active   44m
kube-public       Active   44m
kube-system       Active   44m

vagrant@k8s-master1:~$ kubectl get pod --all-namespaces
NAMESPACE     NAME                                  READY   STATUS    RESTARTS   AGE
kube-system   coredns-f9fd979d6-mvd8f               0/1     Pending   0          43m
kube-system   coredns-f9fd979d6-p899v               0/1     Pending   0          43m
kube-system   etcd-k8s-master1                      1/1     Running   0          44m
kube-system   kube-apiserver-k8s-master1            1/1     Running   0          44m
kube-system   kube-controller-manager-k8s-master1   1/1     Running   0          44m
kube-system   kube-proxy-2pcvd                      1/1     Running   0          43m
kube-system   kube-scheduler-k8s-master1            1/1     Running   0          44m

vagrant@k8s-master1:~$ ls /etc/kubernetes/
admin.conf  controller-manager.conf  kubelet.conf  manifests  pki  scheduler.conf

vagrant@k8s-master1:~$ kubectl get nodes
NAME          STATUS     ROLES    AGE   VERSION
k8s-master1   NotReady   master   53m   v1.19.1
```

从上面可以看到控制面板的组件：etcd，kube-apiserver，kube-controller-manager，kube-scheduler 以及插件 CoreDNS，均采用静态容器模式部署，其部署清单在主机的/etc/kubernetes/manifests 目录里，kubelet 会自动加载此目录并启动 pod。

k8s-master1 的状态还是 NotReady，因为网络还没有部署，K8s 有 flannel，calico 等网络选项，选择部署 calico 网络。

-   部署 pod 网络

```bash
vagrant@k8s-master1:~$ kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

vagrant@k8s-master1:~$ kubectl get ds,deploy,pods -n kube-system
NAME                         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
daemonset.apps/calico-node   1         1         1       1            1           kubernetes.io/os=linux   4m16s
daemonset.apps/kube-proxy    1         1         1       1            1           kubernetes.io/os=linux   76m

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/calico-kube-controllers   1/1     1            1           4m16s
deployment.apps/coredns                   2/2     2            2           76m

NAME                                          READY   STATUS    RESTARTS   AGE
pod/calico-kube-controllers-c9784d67d-mdtzh   1/1     Running   0          4m15s
pod/calico-node-qrlwg                         1/1     Running   0          4m15s
pod/coredns-f9fd979d6-mvd8f                   1/1     Running   0          75m
pod/coredns-f9fd979d6-p899v                   1/1     Running   0          75m
pod/etcd-k8s-master1                          1/1     Running   0          76m
pod/kube-apiserver-k8s-master1                1/1     Running   0          76m
pod/kube-controller-manager-k8s-master1       1/1     Running   0          76m
pod/kube-proxy-2pcvd                          1/1     Running   0          75m
pod/kube-scheduler-k8s-master1                1/1     Running   0          76m

vagrant@k8s-master1:~$ kubectl get node
NAME          STATUS   ROLES    AGE   VERSION
k8s-master1   Ready    master   81m   v1.19.1
```

k8s-master1 状态变为 Ready

-   加入工作节点：在所有工作节点上，依照上述提示运行

```bash
kubeadm token list
kubeadm join 192.168.100.11:6443 --token virvoc.4q5v8rqvuriyr3wy \
    --discovery-token-ca-cert-hash sha256:ce0f619cf549bef741ebcb8c94f0d7853ea2c11ef7573542920e18ad56f7bf40
```

经过一点点时间，在 k8s-master1 上可以看到所有节点都 Ready：

```bash
vagrant@k8s-master1:~$ kubectl get node
NAME          STATUS   ROLES    AGE   VERSION
k8s-master1   Ready    master   96m   v1.19.1
k8s-worker1   Ready    <none>   82s   v1.19.1
k8s-worker2   Ready    <none>   46s   v1.19.1
```

💡 kubeadm 可以扩展单节点集群至多节点，其扩展流程如下：

![kubeadm join](/images/k8s/kubeadm-join.jpeg#center)

这里不尝试。

## 应用测试

创建一个 Nginx Deployment：

```bash
vagrant@k8s-master1:~$ kubectl create deployment nginx --image=nginx
deployment.apps/nginx created

vagrant@k8s-master1:~$ kubectl expose deployment nginx --port=80 --type=NodePort
service/nginx exposed

vagrant@k8s-master1:~$ kubectl get pod,svc -o wide
NAME                         READY   STATUS    RESTARTS   AGE     IP             NODE          NOMINATED NODE   READINESS GATES
pod/nginx-6799fc88d8-z42zc   1/1     Running   0          2m55s   10.100.126.1   k8s-worker2   <none>           <none>

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE     SELECTOR
service/kubernetes   ClusterIP   10.1.0.1       <none>        443/TCP        141m    <none>
service/nginx        NodePort    10.1.132.185   <none>        80:30869/TCP   2m42s   app=nginx
```

可以看到其位于 k8s-worker2 节点，该节点 IP 为 192.168.100.12，端口暴露为 30869，可以使用浏览器访问该地址。

## 部署 Dashboard

1. 安装

```bash
vagrant@k8s-master1:~$ k apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.4/aio/deploy/recommended.yaml
namespace/kubernetes-dashboard created
serviceaccount/kubernetes-dashboard created
service/kubernetes-dashboard created
secret/kubernetes-dashboard-certs created
secret/kubernetes-dashboard-csrf created
secret/kubernetes-dashboard-key-holder created
configmap/kubernetes-dashboard-settings created
role.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
deployment.apps/kubernetes-dashboard created
service/dashboard-metrics-scraper created
deployment.apps/dashboard-metrics-scraper created

# dashboard endpoint
vagrant@k8s-master1:~$ k -n kubernetes-dashboard describe service kubernetes-dashboard
Name:              kubernetes-dashboard
Namespace:         kubernetes-dashboard
Labels:            k8s-app=kubernetes-dashboard
Annotations:       <none>
Selector:          k8s-app=kubernetes-dashboard
Type:              ClusterIP
IP:                10.0.0.124
Port:              <unset>  443/TCP
TargetPort:        8443/TCP
Endpoints:         10.10.126.1:8443
Session Affinity:  None
Events:            <none>
```

2. NodePort

Endpoints: 10.10.126.1:8443，为了访问，可以采用 port forwarding 或者改为 NodePort，port forwarding 使用下面的命令（访问 k8s-worker2:8000）：

```bash
vagrant@k8s-master1:/opt/k8s$ k -n kubernetes-dashboard get pods -o wide
NAME                                         READY   STATUS    RESTARTS   AGE   IP             NODE          NOMINATED NODE   READINESS GATES
dashboard-metrics-scraper-7b59f7d4df-6f7cl   1/1     Running   0          31m   10.10.194.65   k8s-worker1   <none>           <none>
kubernetes-dashboard-665f4c5ff-s7729         1/1     Running   0          31m   10.10.126.1    k8s-worker2   <none>           <none>

vagrant@k8s-master1:/opt/k8s$ k -n kubernetes-dashboard port-forwarding kubernetes-dashboard-665f4c5ff-s7729 8000:8443
```

改为 NodePort 则编辑 kubernetes-dashboard（访问 k8s-worker2:30303）：

```bash
vagrant@k8s-master1:/opt/k8s$ k -n kubernetes-dashboard edit svc kubernetes-dashboard
```

改行 30，加入行 23：

```yaml
19 spec:
20   clusterIP: 10.0.0.124
21   externalTrafficPolicy: Cluster
22   ports:
23   - nodePort: 30303
24     port: 443
25     protocol: TCP
26     targetPort: 8443
27   selector:
28     k8s-app: kubernetes-dashboard
29   sessionAffinity: None
30   type: NodePort
31 status:
32   loadBalancer: {}
```

```bash
vagrant@k8s-master1:~$ k -n kubernetes-dashboard get pod,svc -o wide
NAME                                             READY   STATUS    RESTARTS   AGE   IP             NODE          NOMINATED NODE   READINESS GATES
pod/dashboard-metrics-scraper-7b59f7d4df-6f7cl   1/1     Running   0          50m   10.10.194.65   k8s-worker1   <none>           <none>
pod/kubernetes-dashboard-665f4c5ff-s7729         1/1     Running   0          50m   10.10.126.1    k8s-worker2   <none>           <none>

NAME                                TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)         AGE   SELECTOR
service/dashboard-metrics-scraper   ClusterIP   10.0.0.67    <none>        8000/TCP        50m   k8s-app=dashboard-metrics-scraper
service/kubernetes-dashboard        NodePort    10.0.0.124   <none>        443:30303/TCP   50m   k8s-app=kubernetes-dashboard
```

3. cert

Dashboard 的 cert 在 Chrome 出错：

```txt
NET::ERR_CERT_INVALID
Subject:
Issuer:
Expires on: 6 Oct 2021
Current date: 6 Oct 2020
PEM encoded chain:
......
```

有几种处理方法，一是手动生成 dashboard 的证书，重新部署；二是修改 Chrome 启动命令（/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --ignore-certificate-errors --ignore-urlfetcher-cert-requests &> /dev/null）；三是看到上述页面时，直接敲入神奇的 `thisisunsafe`

4. dashboard-admin

默认的 service account - kubernetes-dashboard 权限不够，需要创建一个：

```bash
vagrant@k8s-master1:~$ k -n kubernetes-dashboard get sa
NAME              SECRETS   AGE
dashboard-admin   1         17m
default           1         85m
vagrant@k8s-master1:~$ k create sa dashboard-admin -n kube-system
serviceaccount/dashboard-admin created
vagrant@k8s-master1:~$ k create clusterrolebinding dashboard-admin --clusterrole=cluster-admin --serviceaccount=kube-system:dashboard-admin
clusterrolebinding.rbac.authorization.k8s.io/dashboard-admin created
vagrant@k8s-master1:~$ k describe sa dashboard-admin -n kube-system
Name:                dashboard-admin
Namespace:           kube-system
Labels:              <none>
Annotations:         <none>
Image pull secrets:  <none>
Mountable secrets:   dashboard-admin-token-5m5lb
Tokens:              dashboard-admin-token-5m5lb
Events:              <none>
# token
vagrant@k8s-master1:~$ k describe secret dashboard-admin-token-5m5lb -n kube-system
```

拿到 token 后可以用其登录 Dashboard 页面。

![kubernetes dashboard](/images/k8s/k8s-dashboard1.png#center)  
![kubernetes dashboard](/images/k8s/k8s-dashboard2.png#center)

## 部署 Metrics-Server

官网已经提示 metric-server 安装要满足 2 个条件：

-   在 master 节点要能访问 metrics server pod ip（kubeadm 部署默认已经满足该条件，二进制部署需注意要在 master 节点也部署 node 组件）
-   apiserver 启用聚合层支持（kubeadm 默认已经启用，二进制部署需自己启用）

先安装 Helm：

```bash
vagrant@k8s-lb:/opt/k8s$ curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
vagrant@k8s-lb:/opt/k8s$ sudo apt-get install apt-transport-https --yes
vagrant@k8s-lb:/opt/k8s$ echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
vagrant@k8s-lb:/opt/k8s$ sudo apt-get update
vagrant@k8s-lb:/opt/k8s$ sudo apt-get install helm
vagrant@k8s-lb:/opt/k8s$ helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

Helm 的架构：

![helm](/images/k8s/helm.png#center)

通过 Helm 部署 Metrics-Server：

```bash
vagrant@k8s-lb:/opt/k8s$ k create namespace kubernetes-metrics
vagrant@k8s-lb:/opt/k8s$ helm install metrics-server stable/metrics-server --version 2.11.2 --namespace kubernetes-metrics
NAME: metrics-server
LAST DEPLOYED: Tue Oct  6 22:07:22 2020
NAMESPACE: kubernetes-metrics
STATUS: deployed
REVISION: 1
NOTES:
The metric server has been deployed.

In a few minutes you should be able to list metrics using the following
command:

  kubectl get --raw "/apis/metrics.k8s.io/v1beta1/nodes"
vagrant@k8s-lb:/opt/k8s$ k get --raw "/apis/metrics.k8s.io/v1beta1/nodes"
{"kind":"NodeMetricsList","apiVersion":"metrics.k8s.io/v1beta1","metadata":{"selfLink":"/apis/metrics.k8s.io/v1beta1/nodes"},"items":[]}

# 增加两个参数：跳过证书验证和使用node ip通信
# --kubelet-insecure-tls
# --kubelet-preferred-address-types=InternalIP
vagrant@k8s-lb:/opt/k8s$ k -n kubernetes-metrics edit deployment metrics-server

# --enable-aggregator-routing=true
vagrant@k8s-master1:/etc/kubernetes/manifests$ sudo vi kube-apiserver.yaml
vagrant@k8s-master1:/etc/kubernetes/manifests$ sudo kubectl apply -f kube-apiserver.yaml

vagrant@k8s-lb:/opt/k8s$ k -n kubernetes-metrics top node
NAME          CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
k8s-master1   128m         6%     1228Mi          64%
k8s-worker1   52m          2%     683Mi           35%
k8s-worker2   65m          3%     785Mi           41%
```

## 删除集群

-   删除一个工作/控制节点：

```bash
kubectl drain <node name> --delete-local-data --force --ignore-daemonsets
sudo kubeadm reset
kubectl delete node <node name>
```

-   reset 不会重置 iptables 或 IPVS，需手动：

```bash
iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X
ipvsadm -C
```

-   如果使用外部 etcd，也需要使用 etcdctl 这样的工具手动删除

-   删除软件：

```bash
sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube\*
sudo apt-get autoremove
sudo rm -rf ~/.kube
```

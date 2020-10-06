+++
title = "Kubernetes 安装 - THW"
date = 2020-10-05T04:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = true
+++

亲手撸一遍

<!--more-->

## 概述

### Kubernetes 集群

Kubernetes 作为容器集群系统，通过健康检查+重启策略实现了 Pod 故障自我修复能力，通过调度算法实现将 Pod 分布式部署，并保持预期副本数，根据负载节点失效状态自动在其他节点拉起 Pod，实现了应用层的高可用性。

针对 Kubernetes 集群，高可用性还应包含以下两个层面的考虑：etcd 数据库的高可用性和 Kubernetes 控制节点组件的高可用性。etcd 将采用 3 个节点单独组建集群实现高可用。控制节点/Master 扮演着总控中心的角色，通过不断与负载节点上的 kubelet 和 kube-proxy 进行通信来维护整个集群的健康工作状态。如果控制节点故障，将无法使用 kubectl 工具或者 API 做任何集群管理。控制节点主要有三个服务 kube-apiserver、kube-controller-manager 和 kube-scheduler，其中 kube-controller-manager 和 kube-scheduler 组件自身通过选择机制已经实现了高可用，所以控制节点高可用主要针对 kube-apiserver 组件，而该组件是以 HTTP API 提供服务，因此对其高可用与 Web 服务器类似，增加负载均衡器对其负载均衡即可，并且可水平扩容。

集群架构如下：  
![k8s cluster](/images/k8s/multi-master.jpg#center)

```md
-   高可用 K8s 集群：两个控制节点（control panel/master）；两个负载节点（worker）
-   入口负载均衡为单节点 Ngnix，生产环境可结合 keepalive 做集群，或者直接使用 AWS，Azure，GCP 提供的负载均衡服务
-   Ubuntu xenial64
-   Kubernetes 1.19.1
-   Docker 5.19
```

| 节点（角色） | IP                 | 组件                                                              |
| ------------ | ------------------ | ----------------------------------------------------------------- |
| k8s\-master1 | 192\.168\.100\.11  | kube\-apiserver，kube\-controller\-manager，kube\-scheduler，etcd |
| k8s\-master2 | 192\.168\.100\.12  | kube\-apiserver，kube\-controller\-manager，kube\-scheduler，etcd |
| k8s\-node1   | 192\.168\.100\.21  | kubelet，kube\-proxy，docker                                      |
| k8s\-node2   | 192\.168\.100\.22  | kubelet，kube\-proxy，docker                                      |
| k8s\-lb      | 192\.168\.100\.100 | Nginx， etcd                                                      |

这里采用 Ngnix 做负载均衡器（另一选择可以是 HAProxy），考虑到 VM 数量，不对其做高可用，生产环境可以结合 keepalive 实施 Ngnix 高可用。

参考下图：  
![api-server HA](/images/k8s/apiserver-ha.png#center)

Kubernetes 中使用到的 CA 和证书：

| 组件            | 证书                                                         |
| --------------- | ------------------------------------------------------------ |
| etcd            | ca\.pem, ca\-key\.pem, server\.pem, server\-key\.pem         |
| kube\-apiserver | ca\.pem, ca\-key\.pem, kubernetes\.pem, kubernetes\-key\.pem |
| kubelet         | ca\.pem, ca\-key\.pem                                        |
| kube\-proxy     | ca\.pem, kube\-proxy\.pem, kube\-proxy\-key\.pem             |
| kubectl         | ca\.pem, k8s\-admin\.pem, k8s\-admin\-key\.pem               |

证书、TLS Bootstrapping、Service Account Token 可参考文末。

部署的主要流程：

1. VM
2. etcd 集群
3. nginx 负载均衡
4. 控制节点 k8s-master1（apiserver，controller-manager，scheduler）
5. 负载节点 k8s-worker1（kubelet，kube-proxy）
6. 加入负载节点 k8s-worker2
7. 加入控制节点 k8s-master2

## VM

和 kubeadm 的一样：<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp;[Kubernetes 安装 - kubeadm](/posts/k8s-install-kubeadm)

```bash
[I] ➜ vagrant status
Current machine states:

k8s-master1               running (virtualbox)
k8s-master2               running (virtualbox)
k8s-worker1               running (virtualbox)
k8s-worker2               running (virtualbox)
k8s-lb                    running (virtualbox)
```

## 部署 etcd 集群

单独部署 etcd 集群，为了节省节点，复用 k8s 节点：k8s-master1，k8s-master2，k8s-lb，必须奇数，这里使用 3 台组建集群，可容忍 1 台机器故障，如果使用 5 台组建集群，可容忍 2 台机器故障。也可以独立于 k8s 集群之外部署，只要 apiserver 能连接到就行。

1. 用自签 CA 生成 etcd 证书
2. k8s-master1：下载并安装 etcd
3. 把 k8s-master1 上的文件分别拷到 k8s-master2，k8s-lb 并启动 etcd
4. 查看 etcd 集群状态

### etcd 证书

```bash
# cfssl 是一个开源的证书管理工具，使用 json 文件生成证书，相比 openssl 更方便使用
vagrant@k8s-master1:~$ mkdir download
vagrant@k8s-master1:~$ cd download
vagrant@k8s-master1:~/download$ wget https://pkg.cfssl.org/R1.2/cfssl_linux-amd64
vagrant@k8s-master1:~/download$ wget https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64
vagrant@k8s-master1:~/download$ wget https://pkg.cfssl.org/R1.2/cfssl-certinfo_linux-amd64

# 添加执行权限
vagrant@k8s-master1:~/download$ chmod +x cfssl_linux-amd64 cfssljson_linux-amd64 cfssl-certinfo_linux-amd64

# 移动到 /usr/local/bin 目录下
vagrant@k8s-master1:~/download$ su
Password:
root@k8s-master1:~/download# mv cfssl_linux-amd64 /usr/local/bin/cfssl
root@k8s-master1:~/download# mv cfssljson_linux-amd64 /usr/local/bin/cfssljson
root@k8s-master1:~/download# mv cfssl-certinfo_linux-amd64 /usr/bin/cfssl-certinfo

# 创建目录用于存放 SSL 证书
root@k8s-master1:~/download# mkdir -p ~/TLS/{etcd,k8s}
root@k8s-master1:~/download# cd ~/TLS/etcd

root@k8s-master1:~/TLS/etcd# cat > ca-config.json << EOF
{
  "signing": {
    "default": {
      "expiry": "87600h"
    },
    "profiles": {
      "www": {
         "expiry": "87600h",
         "usages": [
            "signing",
            "key encipherment",
            "server auth",
            "client auth"
        ]
      }
    }
  }
}
EOF

root@k8s-master1:~/TLS/etcd#  cat > ca-csr.json << EOF
{
    "CN": "etcd cluster",
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "L": "Singapore",
            "ST": "Singapore",
            "O": "kubernetes",
            "OU": "etcd"
        }
    ]
}
EOF

# 生成CA证书和私钥
root@k8s-master1:~/TLS/etcd#  cfssl gencert -initca ca-csr.json | cfssljson -bare ca -
root@k8s-master1:~/TLS/etcd#  ls
ca-config.json  ca.csr  ca-csr.json  ca-key.pem  ca.pem

# 使用自签CA签发etcd HTTPS证书
# hosts 字段中 IP 为所有 etcd 节点的集群内部通信 IP，一个都不能少！为了方便后期扩容可以多写几个预留的 IP
root@k8s-master1:~/TLS/etcd# cat > server-csr.json << EOF
{
    "CN": "etcd",
    "hosts": [
    "192.168.100.11",
    "192.168.100.12",
    "192.168.100.13",
    "192.168.100.14",
    "192.168.100.15",
    "192.168.100.100"
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "L": "Singapore",
            "ST": "Singapore"
        }
    ]
}
EOF

# 生成etcd证书和私钥
root@k8s-master1:~/TLS/etcd# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=www server-csr.json | cfssljson -bare server

root@k8s-master1:~/TLS/etcd# mkdir /opt/etcd/{bin,cfg,ssl} -p
root@k8s-master1:~/TLS/etcd# cd /opt/etcd/
root@k8s-master1:/opt/etcd# cp ~/TLS/etcd/*.pem ~/TLS/etcd/server*pem /opt/etcd/ssl/
```

### k8s-master1

```bash
root@k8s-master1:/opt/etcd# wget https://github.com/etcd-io/etcd/releases/download/v3.4.13/etcd-v3.4.13-linux-amd64.tar.gz
root@k8s-master1:/opt/etcd# tar zxvf etcd-v3.4.13-linux-amd64.tar.gz
root@k8s-master1:/opt/etcd# cp etcd-v3.4.13-linux-amd64/{etcd,etcdctl} /opt/etcd/bin/
root@k8s-master1:/opt/etcd# rm etcd-v3.4.13-linux-amd64.tar.gz

# 创建etcd配置文件
root@k8s-master1:/opt/etcd# cat > /opt/etcd/cfg/etcd.conf << EOF
#[Member]
ETCD_NAME="etcd-1"
ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
ETCD_LISTEN_PEER_URLS="https://192.168.100.11:2380"
ETCD_LISTEN_CLIENT_URLS="https://192.168.100.11:2379"

#[Clustering]
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://192.168.100.11:2380"
ETCD_ADVERTISE_CLIENT_URLS="https://192.168.100.11:2379"
ETCD_INITIAL_CLUSTER="etcd-1=https://192.168.100.11:2380,etcd-2=https://192.168.100.12:2380,etcd-3=https://192.168.100.100:2380"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"
EOF

root@k8s-master1:/opt/etcd# cat > /etc/systemd/system/etcd.service << EOF
[Unit]
Description=Etcd Server
After=network.target
After=network-online.target
Wants=network-online.target

[Service]
Type=notify
EnvironmentFile=/opt/etcd/cfg/etcd.conf
ExecStart=/opt/etcd/bin/etcd \
--cert-file=/opt/etcd/ssl/server.pem \          # 对外提供服务的服务器证书
--key-file=/opt/etcd/ssl/server-key.pem \       # 服务器证书对应的私钥
--peer-cert-file=/opt/etcd/ssl/server.pem \     # peer 证书，用于 etcd 节点之间的相互访问
--peer-key-file=/opt/etcd/ssl/server-key.pem \  # peer 证书对应的私钥
--trusted-ca-file=/opt/etcd/ssl/ca.pem \        # 用于验证访问 etcd 服务器的客户端证书的 CA 根证书
--peer-trusted-ca-file=/opt/etcd/ssl/ca.pem \   # 用于验证 peer 证书的 CA 根证书
--logger=zap
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
EOF

root@k8s-master1:/opt/etcd# systemctl daemon-reload
root@k8s-master1:/opt/etcd# systemctl start etcd
root@k8s-master1:/opt/etcd# systemctl enable etcd
```

### k8s-master2，k8s-lb

把 k8s-master1 上的文件分别拷到 k8s-master2，k8s-lb 并启动 etcd

```bash
# k8s-master1
root@k8s-master1:/opt/etcd# scp -r /opt/etcd/ root@192.168.100.12:/opt/
root@k8s-master1:/opt/etcd# scp /etc/systemd/system/etcd.service root@192.168.100.12:/etc/systemd/system/

root@k8s-master1:/opt/etcd# scp -r /opt/etcd/ root@192.168.100.100:/opt/
root@k8s-master1:/opt/etcd# scp /etc/systemd/system/etcd.service root@192.168.100.100:/etc/systemd/system/

# k8s-master2，修改etcd.conf并启动etcd
root@k8s-master2:/opt/etcd# cat /opt/etcd/cfg/etcd.conf
#[Member]
ETCD_NAME="etcd-2"
ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
ETCD_LISTEN_PEER_URLS="https://192.168.100.12:2380"
ETCD_LISTEN_CLIENT_URLS="https://192.168.100.12:2379"

#[Clustering]
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://192.168.100.12:2380"
ETCD_ADVERTISE_CLIENT_URLS="https://192.168.100.12:2379"
ETCD_INITIAL_CLUSTER="etcd-1=https://192.168.100.11:2380,etcd-2=https://192.168.100.12:2380,etcd-3=https://192.168.100.100:2380"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"

root@k8s-master2:/opt/etcd# systemctl daemon-reload
root@k8s-master2:/opt/etcd# systemctl restart etcd
root@k8s-master2:/opt/etcd# systemctl enable etcd
Created symlink from /etc/systemd/system/multi-user.target.wants/etcd.service to /etc/systemd/system/etcd.service.

# k8s-lb
root@k8s-lb:/opt/etcd# cat /opt/etcd/cfg/etcd.conf
#[Member]
ETCD_NAME="etcd-3"
ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
ETCD_LISTEN_PEER_URLS="https://192.168.100.100:2380"
ETCD_LISTEN_CLIENT_URLS="https://192.168.100.100:2379"

#[Clustering]
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://192.168.100.100:2380"
ETCD_ADVERTISE_CLIENT_URLS="https://192.168.100.100:2379"
ETCD_INITIAL_CLUSTER="etcd-1=https://192.168.100.11:2380,etcd-2=https://192.168.100.12:2380,etcd-3=https://192.168.100.100:2380"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"

root@k8s-lb:/opt/etcd# systemctl daemon-reload
root@k8s-lb:/opt/etcd# systemctl restart etcd
root@k8s-lb:/opt/etcd# systemctl enable etcd
Created symlink from /etc/systemd/system/multi-user.target.wants/etcd.service to /etc/systemd/system/etcd.service.
```

### 查看 etcd 集群状态

```bash
root@k8s-master1:/opt/etcd# source /opt/kubernetes/env.sh
root@k8s-master1:/opt/etcd# ETCDCTL_API=3 /opt/etcd/bin/etcdctl --cacert=/opt/etcd/ssl/ca.pem --cert=/opt/etcd/ssl/server.pem --key=/opt/etcd/ssl/server-key.pem --endpoints="https://192.168.100.11:2379,https://192.168.100.12:2379,https://192.168.100.100:2379" endpoint health
https://192.168.100.12:2379 is healthy: successfully committed proposal: took = 16.199751ms
https://192.168.100.100:2379 is healthy: successfully committed proposal: took = 24.116794ms
https://192.168.100.11:2379 is healthy: successfully committed proposal: took = 27.114111ms
```

## 部署 Nginx 负载均衡器

Nginx 是一个主流 Web 服务和反向代理服务器，这里用四层实现对 apiserver 实现负载均衡。

```bash
root@k8s-lb:~# apt-get install nginx -y
root@k8s-lb:~# cat > /etc/nginx/nginx.conf << "EOF"
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

stream {

    upstream k8s-apiserver {
       server 192.168.100.11:6443;   # k8s-master1
       server 192.168.100.12:6443;   # k8s-master2
    }

    server {
       listen 6443;
       proxy_pass k8s-apiserver;
    }
}

http {

    access_log  /var/log/nginx/access.log;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    server {
        listen       80 default_server;
        server_name  _;

        location / {
        }
    }
}
EOF

root@k8s-lb:~# groupadd nginx
root@k8s-lb:~# useradd nginx -g nginx -s /sbin/nologin -M
root@k8s-lb:~# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@k8s-lb:~# systemctl daemon-reload
root@k8s-lb:~# systemctl start nginx
root@k8s-lb:~# systemctl enable nginx
```

## 部署控制节点 k8s-master1

1. 下载二进制文件并安装
2. 用自签 CA 生成 apiserver 证书
3. 部署 kube-apiserver
4. 部署 kube-controller-manager
5. 部署 kube-scheduler
6. 查看 k8s-master1 状态

### 二进制文件

下载 binaries：[https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.19.md](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.19.md)

```bash
root@k8s-master1:~/TLS/k8s# cd /opt/kubernetes/
root@k8s-master1:/opt/kubernetes# wget https://dl.k8s.io/v1.19.2/kubernetes-server-linux-amd64.tar.gz
root@k8s-master1:/opt/kubernetes# tar zxvf kubernetes-server-linux-amd64.tar.gz
root@k8s-master1:/opt/kubernetes# rm kubernetes-server-linux-amd64.tar.gz
root@k8s-master1:/opt/kubernetes# cd kubernetes/server/bin
root@k8s-master1:/opt/kubernetes# cp kube-apiserver kube-scheduler kube-controller-manager /opt/kubernetes/bin
root@k8s-master1:/opt/kubernetes# cp kubectl /usr/bin/
```

### apiserver 证书

```bash
root@k8s-master1:~# cd ~/TLS/k8s/

### CA根证书
root@k8s-master1:~/TLS/k8s# cat > ca-config.json << EOF
{
  "signing": {
    "default": {
      "expiry": "87600h"
    },
    "profiles": {
      "kubernetes": {
         "expiry": "87600h",
         "usages": [
            "signing",
            "key encipherment",
            "server auth",
            "client auth"
        ]
      }
    }
  }
}
EOF

root@k8s-master1:~/TLS/k8s# cat > ca-csr.json << EOF
{
    "CN": "kubernetes-ca",
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "L": "Singapore",
            "ST": "Singapore",
            "O": "kubernetes",
            "OU": "kubernetes"
        }
    ]
}
EOF

# 生成CA证书和私钥
root@k8s-master1:~/TLS/k8s# cfssl gencert -initca ca-csr.json | cfssljson -bare ca -
root@k8s-master1:~/TLS/k8s# ls
ca-config.json  ca.csr  ca-csr.json  ca-key.pem  ca.pem

### 使用自签CA签发 kube-apiserver 证书
# host包含所有 master，lb，lb vip
root@k8s-master1:~/TLS/k8s# cat > kubernetes-csr.json << EOF
{
    "CN": "k8s-master",
    "hosts": [
      "10.0.0.1",
      "127.0.0.1",
      "192.168.100.11",
      "192.168.100.12",
      "192.168.100.13",
      "192.168.100.14",
      "192.168.100.15",
      "192.168.100.100",
      "192.168.100.101",
      "192.168.100.102",
      "kubernetes",
      "kubernetes.default",
      "kubernetes.default.svc",
      "kubernetes.default.svc.cluster",
      "kubernetes.default.svc.cluster.local"
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "L": "Singapore",
            "ST": "Singapore",
            "O": "kubernetes",
            "OU": "kubernetes"
        }
    ]
}
EOF

# 生成apiserver证书和私钥
root@k8s-master1:~/TLS/k8s# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=kubernetes kubernetes-csr.json | cfssljson -bare kubernetes
root@k8s-master1:~/TLS/k8s# cp *.pem /opt/kubernetes/ssl/
```

### 部署 kube-apiserver

配置文件：

```md
-   –logtostderr：启用日志
-   —v：日志等级
-   –log-dir：日志目录
-   –etcd-servers：etcd 集群地址
-   –bind-address：监听地址
-   –secure-port：https 安全端口
-   –advertise-address：集群通告地址
-   –allow-privileged：启用授权
-   –service-cluster-ip-range：Service 虚拟 IP 地址段
-   –enable-admission-plugins：准入控制模块
-   –authorization-mode：认证授权，启用 RBAC 授权和节点自管理
-   –enable-bootstrap-token-auth：启用 TLS bootstrap 机制
-   –token-auth-file：bootstrap token 文件
-   –service-node-port-range：Service nodeport 类型默认分配端口范围
-   –kubelet-client-xxx：apiserver 访问 kubelet 客户端证书
-   –tls-xxx-file：apiserver https 证书
-   –etcd-xxxfile：连接 Etcd 集群证书
-   –audit-log-xxx：审计日志
```

```bash
root@k8s-master1:/opt/kubernetes# source env.sh
# 格式：token，用户名，UID，用户组
root@k8s-master1:/opt/kubernetes# cat > /opt/kubernetes/cfg/token.csv << EOF
${BOOTSTRAP_TOKEN},kubelet-bootstrap,10001,"system:node-bootstrapper"
EOF

root@k8s-master1:/opt/kubernetes# cat > /opt/kubernetes/cfg/kube-apiserver.conf << EOF
KUBE_APISERVER_OPTS="--logtostderr=false \\
--v=2 \\
--log-dir=/opt/kubernetes/logs \\
--etcd-servers=https://192.168.100.11:2379,https://192.168.100.12:2379,https://192.168.100.100:2379 \\
--bind-address=192.168.100.11 \\
--secure-port=6443 \\
--advertise-address=192.168.100.11 \\
--allow-privileged=true \\
--service-cluster-ip-range=10.0.0.0/24 \\
--enable-admission-plugins=NamespaceLifecycle,LimitRanger,ServiceAccount,ResourceQuota,NodeRestriction \\
--authorization-mode=RBAC,Node \\
--enable-bootstrap-token-auth=true \\
--token-auth-file=/opt/kubernetes/cfg/token.csv \\
--service-node-port-range=30000-32767 \\
--kubelet-client-certificate=/opt/kubernetes/ssl/kubernetes.pem \\   # 用于访问 kubelet 的客户端证书
--kubelet-client-key=/opt/kubernetes/ssl/kubernetes-key.pem \\       # 用于访问 kubelet 的客户端证书的私钥
--tls-cert-file=/opt/kubernetes/ssl/kubernetes.pem  \\               # 用于对外提供服务的服务器证书
--tls-private-key-file=/opt/kubernetes/ssl/kubernetes-key.pem \\     # 服务器证书对应的私钥
--client-ca-file=/opt/kubernetes/ssl/ca.pem \\                       # 用于验证访问 kube-apiserver 的客户端的证书的 CA 根证书
--service-account-key-file=/opt/kubernetes/ssl/ca-key.pem \\         # 用于验证 service account token 的公钥
--etcd-cafile=/opt/etcd/ssl/ca.pem \\                                # 用于验证 etcd 服务器证书的 CA 根证书
--etcd-certfile=/opt/etcd/ssl/server.pem \\                          # 用于访问 etcd 的客户端证书
--etcd-keyfile=/opt/etcd/ssl/server-key.pem \\                       # 用于访问 etcd 的客户端证书的私钥
--audit-log-maxage=30 \\
--audit-log-maxbackup=3 \\
--audit-log-maxsize=100 \\
--audit-log-path=/opt/kubernetes/logs/k8s-audit.log"
EOF

root@k8s-master1:/opt/kubernetes# cat > /etc/systemd/system/kube-apiserver.service << EOF
[Unit]
Description=Kubernetes API Server
Documentation=https://github.com/kubernetes/kubernetes

[Service]
EnvironmentFile=/opt/kubernetes/cfg/kube-apiserver.conf
ExecStart=/opt/kubernetes/bin/kube-apiserver \$KUBE_APISERVER_OPTS
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF

root@k8s-master1:/opt/kubernetes# systemctl daemon-reload
root@k8s-master1:/opt/kubernetes# systemctl start kube-apiserver
root@k8s-master1:/opt/kubernetes# systemctl enable kube-apiserver
```

### 部署 kube-controller-manager

```bash
# --cluster-cidr=10.10.0.0/16 # pod network
# --service-cluster-ip-range=10.0.0.0/24 # service netowrk
# --service-account-private-key-file=/opt/kubernetes/ssl/ca-key.pem # 用于对 service account token 进行签名的私钥
root@k8s-master1:/opt/kubernetes# cat > /opt/kubernetes/cfg/kube-controller-manager.conf << EOF
> KUBE_CONTROLLER_MANAGER_OPTS="--logtostderr=false \\
> --v=2 \\
> --log-dir=/opt/kubernetes/logs \\
> --leader-elect=true \\
> --master=127.0.0.1:8080 \\
> --bind-address=127.0.0.1 \\
> --allocate-node-cidrs=true \\
> --cluster-cidr=10.10.0.0/16 \\
> --service-cluster-ip-range=10.0.0.0/24 \\
> --cluster-signing-cert-file=/opt/kubernetes/ssl/ca.pem \\
> --cluster-signing-key-file=/opt/kubernetes/ssl/ca-key.pem  \\
> --root-ca-file=/opt/kubernetes/ssl/ca.pem \\
> --service-account-private-key-file=/opt/kubernetes/ssl/ca-key.pem \\
> --experimental-cluster-signing-duration=87600h0m0s"
> EOF

root@k8s-master1:/opt/kubernetes# cat > /etc/systemd/system/kube-controller-manager.service << EOF
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/kubernetes/kubernetes

[Service]
EnvironmentFile=/opt/kubernetes/cfg/kube-controller-manager.conf
ExecStart=/opt/kubernetes/bin/kube-controller-manager \$KUBE_CONTROLLER_MANAGER_OPTS
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF

root@k8s-master1:/opt/kubernetes# systemctl daemon-reload
root@k8s-master1:/opt/kubernetes# systemctl start kube-controller-manager
root@k8s-master1:/opt/kubernetes# systemctl enable kube-controller-manager
```

### 部署 kube-scheduler

```bash
cat > /opt/kubernetes/cfg/kube-scheduler.conf << EOF
KUBE_SCHEDULER_OPTS="--logtostderr=false \
--v=2 \
--log-dir=/opt/kubernetes/logs \
--leader-elect \
--master=127.0.0.1:8080 \
--bind-address=127.0.0.1"
EOF

root@k8s-master1:/opt/kubernetes# cat > /etc/systemd/system/kube-scheduler.service << EOF
[Unit]
Description=Kubernetes Scheduler
Documentation=https://github.com/kubernetes/kubernetes

[Service]
EnvironmentFile=/opt/kubernetes/cfg/kube-scheduler.conf
ExecStart=/opt/kubernetes/bin/kube-scheduler \$KUBE_SCHEDULER_OPTS
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF

root@k8s-master1:/opt/kubernetes# systemctl daemon-reload
root@k8s-master1:/opt/kubernetes# systemctl start kube-scheduler
root@k8s-master1:/opt/kubernetes# systemctl enable kube-scheduler
```

### 查看 k8s-master1 状态

所有控制节点组件都已经启动成功，通过 kubectl 工具查看当前集群组件状态：

```bash
root@k8s-master1:/opt/kubernetes# kubectl get cs
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE             ERROR
scheduler            Healthy   ok
controller-manager   Healthy   ok
etcd-0               Healthy   {"health":"true"}
etcd-2               Healthy   {"health":"true"}
etcd-1               Healthy   {"health":"true"}
```

## 部署负载节点 k8s-worker1

1. 部署 kubelet
2. 部署 kube-proxy
3. 部署 CNI 网络
4. 授权 apiserver 访问 kubelet

### kubelet

#### TLS Bootstrapping

```bash
# 授权 kubelete-bootstrap 使用证书
root@k8s-master1:/opt/kubernetes# kubectl create clusterrolebinding kubelet-bootstrap \
--clusterrole=system:node-bootstrapper \
--user=kubelet-bootstrap
```

#### 配置文件

```md
-   –hostname-override：显示名称，集群中唯一(本机 ip)
-   –network-plugin：启用 CNI
-   –kubeconfig：指定生成的文件，TLS bootstraping 下会自动生成，后面用于连接 apiserver
-   –bootstrap-kubeconfig：TLS bootstraping 首次启动向 apiserver 申请证书
-   –config：配置参数文件
-   –cert-dir：kubelet 证书生成目录
-   –pod-infra-container-image：Pod 网络容器的镜像
```

```bash
# 从k8s-master1拷贝
root@k8s-worker1:~# scp -r root@k8s-master1:/opt/kubernetes /opt
root@k8s-worker1:/opt/kubernetes# cp kubernetes/server/bin/{kubelet,kube-proxy} /opt/kubernetes/bin
root@k8s-worker1:/opt/kubernetes# cp kubernetes/server/bin/kubectl /usr/bin
root@k8s-worker1:/opt/kubernetes# rm logs/*


# 生成 kubelet bootstrap kubeconfig 配置文件
root@k8s-worker1:/opt/kubernetes# source env.sh
# 设置集群参数
root@k8s-worker1:/opt/kubernetes# kubectl config set-cluster kubernetes \
  --certificate-authority=/opt/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER} \
  --kubeconfig=bootstrap.kubeconfig
# 设置客户端认证参数
root@k8s-worker1:/opt/kubernetes# kubectl config set-credentials "kubelet-bootstrap" \
  --token=${BOOTSTRAP_TOKEN} \
  --kubeconfig=bootstrap.kubeconfig
# 设置上下文参数
root@k8s-worker1:/opt/kubernetes# kubectl config set-context default \
  --cluster=kubernetes \
  --user="kubelet-bootstrap" \
  --kubeconfig=bootstrap.kubeconfig
# 设置默认上下文
root@k8s-worker1:/opt/kubernetes# kubectl config use-context default --kubeconfig=bootstrap.kubeconfig
root@k8s-worker1:/opt/kubernetes# mv bootstrap.kubeconfig /opt/kubernetes/cfg/

# 创建 kubelet 参数配置模板文件
root@k8s-worker1:/opt/kubernetes# cat > /opt/kubernetes/cfg/kubelet-config.yml << EOF
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
address: 0.0.0.0
port: 10250
readOnlyPort: 10255
cgroupDriver: cgroupfs
clusterDNS:
  - 10.0.0.2
clusterDomain: cluster.local
failSwapOn: false
authentication:
  anonymous:
    enabled: false
  webhook:
    cacheTTL: 2m0s
    enabled: true
  x509:
    clientCAFile: /opt/kubernetes/ssl/ca.pem
authorization:
  mode: Webhook
  webhook:
    cacheAuthorizedTTL: 5m0s
    cacheUnauthorizedTTL: 30s
evictionHard:
  imagefs.available: 15%
  memory.available: 100Mi
  nodefs.available: 10%
  nodefs.inodesFree: 5%
maxOpenFiles: 1000000
maxPods: 110
EOF

# kubelet 配置文件
root@k8s-worker1:/opt/kubernetes# cat > /opt/kubernetes/cfg/kubelet.conf << EOF
KUBELET_OPTS="--logtostderr=false \\
--v=2 \\
--log-dir=/opt/kubernetes/logs \\
--hostname-override=k8s-worker1 \\
--network-plugin=cni \\
--kubeconfig=/opt/kubernetes/cfg/kubelet.kubeconfig \\
--bootstrap-kubeconfig=/opt/kubernetes/cfg/bootstrap.kubeconfig \\
--config=/opt/kubernetes/cfg/kubelet-config.yml \\
--cert-dir=/opt/kubernetes/ssl \\
--pod-infra-container-image=gcr.io/google_containers/pause-amd64:3.2"
EOF
```

#### 部署 kubelet

```bash
root@k8s-worker1:/opt/kubernetes# cat > /etc/systemd/system/kubelet.service << EOF
[Unit]
Description=Kubernetes Kubelet
After=docker.service

[Service]
EnvironmentFile=/opt/kubernetes/cfg/kubelet.conf
ExecStart=/opt/kubernetes/bin/kubelet \$KUBELET_OPTS
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
EOF

root@k8s-worker1:/opt/kubernetes# systemctl daemon-reload
root@k8s-worker1:/opt/kubernetes# systemctl start kubelet
root@k8s-worker1:/opt/kubernetes# systemctl enable kubelet
```

#### 批准 kubelet CSR

```bash
root@k8s-master1:/opt/kubernetes# kubectl get csr
NAME                                                   AGE    SIGNERNAME                                    REQUESTOR           CONDITION
node-csr-1P8c9f8ENeYlApuONKY2PIwCFOujL28Q75hCRihPVOA   102s   kubernetes.io/kube-apiserver-client-kubelet   kubelet-bootstrap   Pending
root@k8s-master1:/opt/kubernetes# kubectl certificate approve node-csr-1P8c9f8ENeYlApuONKY2PIwCFOujL28Q75hCRihPVOA
certificatesigningrequest.certificates.k8s.io/node-csr-1P8c9f8ENeYlApuONKY2PIwCFOujL28Q75hCRihPVOA approved
root@k8s-master1:/opt/kubernetes# kubectl get csr
NAME                                                   AGE     SIGNERNAME                                    REQUESTOR           CONDITION
node-csr-1P8c9f8ENeYlApuONKY2PIwCFOujL28Q75hCRihPVOA   2m44s   kubernetes.io/kube-apiserver-client-kubelet   kubelet-bootstrap   Approved,Issued
```

### kube-proxy

#### kube-proxy 证书

```bash
### 使用自签CA签发 kube-proxy 证书
root@k8s-master1:~/TLS/k8s# cat > kube-proxy-csr.json << EOF
{
  "CN": "system:kube-proxy",
  "hosts": [],
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "CN",
      "L": "Singapore",
      "ST": "Singapore",
      "O": "kubernetes",
      "OU": "kubernetes"
    }
  ]
}
EOF

# 生成证书
root@k8s-master1:~/TLS/k8s# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=kubernetes kube-proxy-csr.json | cfssljson -bare kube-proxy
root@k8s-master1:~/TLS/k8s# ls
root@k8s-master1:~/TLS/k8s# cp *.pem /opt/kubernetes/ssl/
root@k8s-master1:~/TLS/k8s# scp -r /opt/kubernetes/ssl root@k8s-worker1:/opt/kubernetes
```

#### 配置文件

```bash
# 生成 kube-proxy kubeconfig 配置文件
root@k8s-worker1:/opt/kubernetes# source env.sh
root@k8s-worker1:/opt/kubernetes# kubectl config set-cluster kubernetes \
  --certificate-authority=/opt/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER} \
  --kubeconfig=kube-proxy.kubeconfig
root@k8s-worker1:/opt/kubernetes# kubectl config set-credentials kube-proxy \
  --client-certificate=/opt/kubernetes/ssl/kube-proxy.pem \
  --client-key=/opt/kubernetes/ssl/kube-proxy-key.pem \
  --embed-certs=true \
  --kubeconfig=kube-proxy.kubeconfig
root@k8s-worker1:/opt/kubernetes# kubectl config set-context default \
  --cluster=kubernetes \
  --user=kube-proxy \
  --kubeconfig=kube-proxy.kubeconfig
root@k8s-worker1:/opt/kubernetes# kubectl config use-context default --kubeconfig=kube-proxy.kubeconfig
root@k8s-worker1:/opt/kubernetes# mv kube-proxy.kubeconfig cfg

root@k8s-worker1:/opt/kubernetes# cat > /opt/kubernetes/cfg/kube-proxy-config.yml << EOF
kind: KubeProxyConfiguration
apiVersion: kubeproxy.config.k8s.io/v1alpha1
bindAddress: 0.0.0.0
metricsBindAddress: 0.0.0.0:10249
clientConnection:
  kubeconfig: /opt/kubernetes/cfg/kube-proxy.kubeconfig
hostnameOverride: k8s-worker1
clusterCIDR: 10.0.0.0/24
EOF

# kube-proxy 配置文件
root@k8s-worker1:/opt/kubernetes# cat > /opt/kubernetes/cfg/kube-proxy.conf << EOF
KUBE_PROXY_OPTS="--logtostderr=false \\
--v=2 \\
--log-dir=/opt/kubernetes/logs \\
--config=/opt/kubernetes/cfg/kube-proxy-config.yml"
EOF
```

#### 部署 kube-proxy

```bash
root@k8s-worker1:/opt/kubernetes# cat > /etc/systemd/system/kube-proxy.service << EOF
[Unit]
Description=Kubernetes Proxy
After=network.target

[Service]
EnvironmentFile=/opt/kubernetes/cfg/kube-proxy.conf
ExecStart=/opt/kubernetes/bin/kube-proxy \$KUBE_PROXY_OPTS
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
EOF

root@k8s-worker1:/opt/kubernetes# systemctl daemon-reload
root@k8s-worker1:/opt/kubernetes# systemctl start kubelet
root@k8s-worker1:/opt/kubernetes# systemctl enable kubelet
```

### 部署 CNI 网络

Kubernetes 通过 CNI 支持多种网络方案，其中最广泛使用的是 flannel 和 calico，flannel 安装没有成功，使用 calico：

```bash
# calico
root@k8s-master1:/opt/kubernetes# kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
root@k8s-master1:/opt/kubernetes# kubectl get ds,deploy,pods -n kube-system

NAME                         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
daemonset.apps/calico-node   1         1         0       1            0           kubernetes.io/os=linux   3m10s

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/calico-kube-controllers   0/1     1            0           3m10s

NAME                                          READY   STATUS              RESTARTS   AGE
pod/calico-kube-controllers-c9784d67d-npjhl   0/1     ContainerCreating   0          3m9s
pod/calico-node-xz27d                         0/1     Running             2          3m9s
vagrant@k8s-master1:/opt/kubernetes$ kubectl get cs,nodes
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                                 STATUS    MESSAGE             ERROR
componentstatus/controller-manager   Healthy   ok
componentstatus/scheduler            Healthy   ok
componentstatus/etcd-2               Healthy   {"health":"true"}
componentstatus/etcd-1               Healthy   {"health":"true"}
componentstatus/etcd-0               Healthy   {"health":"true"}

NAME               STATUS   ROLES    AGE     VERSION
node/k8s-worker1   Ready    <none>   2d20h   v1.19.2
```

### 授权 apiserver 访问 kubelet

```bash
cat > cfg/apiserver-to-kubelet-rbac.yaml << EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
  labels:
    kubernetes.io/bootstrapping: rbac-defaults
  name: system:kube-apiserver-to-kubelet
rules:
  - apiGroups:
      - ""
    resources:
      - nodes/proxy
      - nodes/stats
      - nodes/log
      - nodes/spec
      - nodes/metrics
      - pods/log
    verbs:
      - "*"
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: system:kube-apiserver
  namespace: ""
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:kube-apiserver-to-kubelet
subjects:
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: kubernetes
EOF
```

## 增加负载节点 k8s-worker2

```bash
# 拷贝文件
root@k8s-worker2:~# scp -r root@k8s-worker1:/opt/kubernetes /opt
root@k8s-worker2:~# scp -r root@k8s-worker1:/etc/systemd/system/{kubelet,kube-proxy}.service /etc/systemd/system

# 删除TLS bootstrapping证书，每个node都不同
root@k8s-worker2:/opt/kubernetes# rm cfg/kubelet.kubeconfig
root@k8s-worker2:/opt/kubernetes# rm ssl/kubelet*
root@k8s-worker2:/opt/kubernetes# rm logs/*

# 修改配置
# --hostname-override=k8s-worker2
root@k8s-worker2:/opt/kubernetes# vi /opt/kubernetes/cfg/kubelet.conf
# hostnameOverride: k8s-worker
root@k8s-worker2:/opt/kubernetes# vi /opt/kubernetes/cfg/kube-proxy-config.yml

# 启动 service
root@k8s-worker2:/opt/kubernetes# systemctl daemon-reload
root@k8s-worker2:/opt/kubernetes# systemctl start kubelet
root@k8s-worker2:/opt/kubernetes# systemctl enable kubelet
root@k8s-worker2:/opt/kubernetes# systemctl start kube-proxy
root@k8s-worker2:/opt/kubernetes# systemctl enable kube-proxy
```

在 Master 上批准新 kubelet 证书申请：

```bash
root@k8s-master1:/opt/kubernetes# kubectl get csr
NAME                                                   AGE   SIGNERNAME                                    REQUESTOR           CONDITION
node-csr-eGmfYzEEAen63kmzgW8EfOQ9APWaQEzWlsJ8QY-czmo   96s   kubernetes.io/kube-apiserver-client-kubelet   kubelet-bootstrap   Pending
root@k8s-master1:/opt/kubernetes# kubectl certificate approve node-csr-eGmfYzEEAen63kmzgW8EfOQ9APWaQEzWlsJ8QY-czmo
certificatesigningrequest.certificates.k8s.io/node-csr-eGmfYzEEAen63kmzgW8EfOQ9APWaQEzWlsJ8QY-czmo approved
```

## 增加控制节点 k8s-master2

```bash
# 拷贝文件
root@k8s-master2:~# scp -r root@k8s-master1:/opt/kubernetes /opt
root@k8s-master2:~# scp -r root@k8s-master1:/etc/systemd/system/kube* /etc/systemd/system
root@k8s-master2:~# scp -r root@k8s-master1:/usr/bin/kubectl /usr/bin

# 修改配置
# --bind-address=192.168.100.12 \
# --advertise-address=192.168.100.12 \
root@k8s-master2:/opt/kubernetes# vi /opt/kubernetes/cfg/kube-apiserver.conf

# 启动 service
root@k8s-master2:/opt/kubernetes# systemctl daemon-reload
root@k8s-master2:/opt/kubernetes# systemctl start kube-apiserver
root@k8s-master2:/opt/kubernetes# systemctl status kube-apiserver
root@k8s-master2:/opt/kubernetes# systemctl enable kube-apiserver
root@k8s-master2:/opt/kubernetes# systemctl start kube-controller-manager
root@k8s-master2:/opt/kubernetes# systemctl status kube-controller-manager
root@k8s-master2:/opt/kubernetes# systemctl enable kube-controller-manager
root@k8s-master2:/opt/kubernetes# systemctl start kube-scheduler
root@k8s-master2:/opt/kubernetes# systemctl status kube-scheduler
root@k8s-master2:/opt/kubernetes# systemctl enable kube-scheduler

root@k8s-master2:/opt/kubernetes# kubectl get cs,node
```

## 部署 CoreDNS

CoreDNS 用于集群内部 Service 名称解析。总体流程是使用 deploy.sh 生成 yaml 并保存成 coredns.yaml 文件并执行 kubectl apply 进行部署：

```
root@k8s-master1:/opt/coredns# git clone https://github.com/coredns/deployment.git
root@k8s-master1:/opt/coredns# apt install -y jq
root@k8s-master1:/opt/coredns# deployment/kubernetes/deploy.sh -i 10.0.0.2 > coredns.yaml
root@k8s-master1:/opt/coredns#  kubectl apply -f coredns.yaml
root@k8s-master1:/opt/coredns# kubectl get pods -n kube-system
NAME                                      READY   STATUS              RESTARTS   AGE
calico-kube-controllers-c9784d67d-npjhl   0/1     ContainerCreating   0          6d21h
calico-node-r6s2j                         0/1     CrashLoopBackOff    362        4d1h
calico-node-xz27d                         0/1     CrashLoopBackOff    613        6d21h
coredns-7bf4bd64bd-4dsvk                  0/1     ContainerCreating   0          62s
```

## 部署 Metrics

## 部署 Dashboard

默认 Dashboard 只能集群内部访问，修改 Service 为 NodePort 类型，暴露到外部：

## 安装和配置 kubectl

### 配置 kubectl

kubectl 使用 https 和 apiserver 通信，kubectl 用于集群管理，所以创建具有最搞权限的 admin 证书:

-   O: system:masters：kube-apiserver 收到使用该证书的客户端请求后，为请求添加组（Group）认证标识 system:masters
-   预定义的 ClusterRoleBinding cluster-admin 将 Group system:masters 与 Role cluster-admin 绑定，该 Role 授予操作集群所需的最高权限；
-   该证书只会被 kubectl 当做 client 证书使用，所以 hosts 字段为空；

```bash
### 使用自签CA签发 admin 证书
root@k8s-master1:~/TLS/k8s# cat > k8s-admin-csr.json <<EOF
{
  "CN": "k8s-admin",
  "hosts": [],
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "CN",
      "ST": "Singapore",
      "L": "Singapore",
      "O": "system:masters",
      "OU": "kubernetes"
    }
  ]
}
EOF

# 生成admin证书和私钥
root@k8s-master1:~/TLS/k8s# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=kubernetes k8s-admin-csr.json | cfssljson -bare k8s-admin

root@k8s-master1:~/TLS/k8s# ls
root@k8s-master1:~/TLS/k8s# cp *.pem /opt/kubernetes/ssl/

# 设置集群参数
root@k8s-master1:/opt/kubernetes# kubectl config set-cluster k8s-thw \
  --certificate-authority=/opt/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER} \
  --kubeconfig=kubectl.kubeconfig

# 设置客户端认证参数
root@k8s-master1:/opt/kubernetes# kubectl config set-credentials k8s-admin \
  --client-certificate=/opt/kubernetes/ssl/k8s-admin.pem \
  --client-key=/opt/kubernetes/ssl/k8s-admin-key.pem \
  --embed-certs=true \
  --kubeconfig=kubectl.kubeconfig

# 设置上下文参数
root@k8s-master1:/opt/kubernetes# kubectl config set-context k8s-thw \
  --cluster=k8s-thw \
  --user=k8s-admin \
  --kubeconfig=kubectl.kubeconfig

# 设置默认上下文
root@k8s-master1:/opt/kubernetes# kubectl config use-context k8s-thw --kubeconfig=kubectl.kubeconfig
root@k8s-master1:/opt/kubernetes# mv kubectl.kubeconfig /opt/kubernetes/cfg
```

拷贝到 k8s-lb 上使用：

```bash
root@k8s-lb:/opt# scp -r root@k8s-master1:/opt/kubernetes /opt
root@k8s-lb:/opt/kubernetes# cp kubernetes/server/bin/kubectl /usr/bin
root@k8s-lb:/opt/kubernetes# cp cfg/kubectl.kubeconfig ~/.kube/
root@k8s-lb:/opt/kubernetes# mv ~/.kube/kubectl.kubeconfig ~/.kube/config
root@k8s-lb:/opt/kubernetes# kubectl cluster-info
Kubernetes master is running at https://192.168.100.100:6443
CoreDNS is running at https://192.168.100.100:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

## e2e test

## 附：Kubernetes 中使用到的 CA 和证书

K8s 各个组件提供的接口中包含了集群的内部信息。如果这些接口被非法访问，将影响集群的安全，因此组件之间的通信需要采用<span style="color:orange">双向 TLS 认证</span>。即客户端和服务器端都需要验证对方的身份信息，以避免恶意第三方伪造身份窃取信息或者对系统进行攻击。为了相互验证对方的身份，通信双方中的任何一方都需要做下面两件事情：

-   向对方提供标明自己身份的一个证书
-   验证对方提供的身份证书是否合法，是否伪造的？

数字证书的验证是在协议层面通过 TLS 完成的，除了需要在建立通信时提供相关的证书和密钥外，在应用层面并不需要进行特殊处理。在进行双向认证时，会涉及到下面这些证书相关的文件：

```md
-   服务器端证书：服务器用于证明自身身份的数字证书，里面主要包含了服务器端的公钥以及服务器的身份信息。
-   服务器端私钥：服务器端证书中包含的公钥所对应的私钥。公钥和私钥是成对使用的，在进行 TLS 验证时，服务器使用该私钥来向客户端证明自己是服务器端证书的拥有者。
-   客户端证书：客户端用于证明自身身份的数字证书，里面主要包含了客户端的公钥以及客户端的身份信息。
-   客户端私钥：客户端证书中包含的公钥所对应的私钥，同理，客户端使用该私钥来向服务器端证明自己是客户端证书的拥有者。
-   服务器端 CA 根证书：签发服务器端证书的 CA 根证书，客户端使用该 CA 根证书来验证服务器端证书的合法性。
-   客户端端 CA 根证书：签发客户端证书的 CA 根证书，服务器端使用该 CA 根证书来验证客户端证书的合法性。
```

![k8s certificates](/images/k8s/k8s-cert.png#center)

图中的箭头表明了组件的调用方向，箭头所指方向为服务提供方，另一头为服务调用方。为了实现 TLS 双向认证，服务提供方需要使用一个服务器证书，服务调用方则需要提供一个客户端证书，并且双方都需要使用一个 CA 证书来验证对方提供的证书。

图中标注的这些证书的作用分别如下：

-   etcd

```txt
1. etcd 集群中各个节点之间相互通信使用的证书，由于一个 etctd 节点既为其他节点提供服务，又需要作为客户端访问其他节点，因此该证书同时用作服务器证书和客户端证书。
2. etcd 集群向外提供服务使用的证书，该证书是服务器证书。
```

-   kube-apiserver

```txt
3.  kube-apiserver 作为客户端访问 etcd 使用的证书，该证书是客户端证书。
4.  kube-apiserver 对外提供服务使用的证书，该证书是服务器证书。
11. kube-apiserver 作为客户端访问 kubelet 采用的证书，该证书是客户端证书。
```

-   kube-controller-manager

```txt
5. kube-controller-manager 作为客户端访问 kube-apiserver 使用的证书，该证书是客户端证书。
12. kube-controller-manager 用于生成和验证 service-account token 的证书，该证书并不会像其他证书一样用于身份认证，而是将证书中的公钥/私钥对用于 service account token 的生成和验证。
```

-   kube-scheduler

```txt
6. kube-scheduler 作为客户端访问 kube-apiserver 使用的证书，该证书是客户端证书。
```

-   kube-proxy

```txt
7.  kube-proxy 作为客户端访问 kube-apiserver 使用的证书，该证书是客户端证书。
```

-   kubelet

```txt
8.  kubelet 作为客户端访问 kube-apiserver 使用的证书，该证书是客户端证书。
10. kubelet 对外提供服务使用的证书，该证书是服务器证书。
```

-   kubectl

```txt
9.  管理员用户通过 kubectl 访问 kube-apiserver 使用的证书，该证书是客户端证书。
```

## 附：TLS Bootstrapping

证书包含了以下字段：
Common name (CN)  
Company (or Organization, O)  
Company division (or Organization Unit, OU)  
Country (C)  
State (S)  
Locality (L) or City

X.509 客户端认证：

-   当服务器要确认客户端时，客户端必须提供客户端证书
-   客户端证书必须是有效 CA 签发的
-   Kubernetes 的 User 来自证书的 CN 字段，Groups 来自证书的 Organization 字段
    -   对于 kube-proxy 其 CN/User 必须是系统默认的"system:kube-proxy"
    -   对于 kubelet 其 Organiation/Group 必须是"system:nodes"而且 CN/User 必须是"system:node:{hostname}"，每个 kubelet 都要有自己的 identity 是因为 Kubernetes 可以使用 Node Authorizer 和 Node Restriction Admission 插件，用来定义和限制 kubelet 读写其所在负载节点上的资源

<span style="color:orange">TLS Bootstrapping 机制</span>既然需要为每一个负载节点上的 Kubelet 分别生成一个证书（system:node:{hostname}），由于负载节点可能很多，手动生成 Kubelet 证书的过程会比较繁琐，apiserver 启用 TLS Bootstrapping 认证机制来自动颁发证书，TLS Bootstraping 工作流程：

![TLS bootstraping](/images/k8s/bootstrap-token.png#center)

1. kube-apiserver 生成一个随机 bootstrap token
2. 将该 bootstrap token 写入到一个 kubeconfig 文件中，作为 kubelet 调用 kube-apiserver 的客户端验证方式
3. 通过<span style="color:orange"> --bootstrap-kubeconfig </span>启动参数将 bootstrap token 传递给 kubelet 进程
4. Kubelet 采用 bootstrap token 调用 kube-apiserver API，kube-apiserver 收到证书签名请求（CSR）请求后，对其中的 Token 进行认证，生成 kubelet 所需的服务器和客户端证书
5. 证书生成后，Kubelet 采用生成的证书和 kube-apiserver 进行通信，并删除本地的 kubeconfig 文件，以避免 bootstrap token 泄漏风险

## 附：Service Account Token

K8s 可以为 pod 指定一个 service account，并为其产生<span style="color:orange">Service Account Token</span>（JWT），然后 pod 中的应用程序就可以用此 token 和 apiserver 通信（获取集群的信息，甚至对集群进行改动）。ServiceAccount 主要包含了三个内容：namespace、Token 和 CA。namespace 指定了 pod 所在的 namespace，CA 用于验证 apiserver 的证书，token 用作身份验证。它们都通过 mount 的方式保存在 pod 的文件系统中。

service account 证书被用于生成和验证 service account token。该证书的用法和前面介绍的其他证书不同，因为实际上使用的是其公钥和私钥，而并不需要对证书进行验证。下图展示了 kubernetes 中生成、使用和验证 service account token 的过程：

![service account token](/images/k8s/service-account-token.png#center)

由此可见，Kubernetes 提供了两种客户端认证的方法，控制面组件采用的是客户端数字证书;而在集群中部署的应用则采用了 service account token 的方式。为什么 Kubernetes 不为 service account 也生成一个证书，并采用该证书进行身份认证呢？

## 附：Ingress

如果要暴露运行在 Kubernetes 里的应用，有三种实现方式：

-   TLS configured for service exposed using Ingress
-   define a secret with a certificate and private key, and reference it in ingress resource
-   kube-lego: auto cert generation using Let's Encrypt

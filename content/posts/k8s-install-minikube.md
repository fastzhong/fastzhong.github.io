+++
title = "Kubernetes 安装 - minikube"
date = 2020-09-03T03:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = false
+++

本地安装 Kubernetes，minikube 是最简易的方式

<!--more-->

参考：  
<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [安装并配置 kubectl](https://kubernetes.io/zh/docs/tasks/tools/install-kubectl/)  
<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [Kubernetes 安装 - minikube](https://kubernetes.io/zh/docs/tasks/tools/install-minikube/)

1. 检查您的 macOS 是否支持虚拟化技术，请运行下面的命令：

```bash
/Volumes/MySpace/k8slab
[I] ➜ sysctl -a | grep -E --color 'machdep.cpu.features|VMX'
machdep.cpu.features: FPU VME DE PSE TSC MSR PAE MCE CX8 APIC SEP MTRR PGE MCA CMOV PAT PSE36 CLFSH DS ACPI MMX FXSR SSE SSE2 SS HTT TM PBE SSE3 PCLMULQDQ DTES64 MON DSCPL VMX EST TM2 SSSE3 FMA CX16 TPR PDCM SSE4.1 SSE4.2 x2APIC MOVBE POPCNT AES PCID XSAVE OSXSAVE SEGLIM64 TSCTMR AVX1.0 RDRAND F16C
```

如果你在输出结果中看到了 VMX （应该会高亮显示）的字眼，说明您的电脑已启用 VT-x 特性。

2. 安装 Hypervisor

我选的是 VirtualBox：

```bash
/Volumes/MySpace/k8slab
[I] ➜ brew cask install virtualbox
```

3. 安装 kubectl

kubectl 是和 K8s apiserver 交互的命令工具

```bash
/Volumes/MySpace/k8slab
[I] ➜ brew install kubectl
```

4. 安装 minikube

```bash
/Volumes/MySpace/k8slab
[I] ➜ brew install minikube

/Volumes/MySpace/k8slab
[I] ➜ brew link minikube
Linking /usr/local/Cellar/minikube/1.13.0... 3 symlinks created

[I] ➜ minikube
minikube provisions and manages local Kubernetes clusters optimized for development workflows.

Basic Commands:
  start          Starts a local Kubernetes cluster
  status         Gets the status of a local Kubernetes cluster
  stop           Stops a running local Kubernetes cluster
  delete         Deletes a local Kubernetes cluster
  dashboard      Access the Kubernetes dashboard running within the minikube cluster
  pause          pause Kubernetes
  unpause        unpause Kubernetes
```

可以看到 minikube 的命令还是相当简洁干脆的

5. 启动 minikube

```bash
/Volumes/MySpace/k8slab
[I] ➜ minikube start --vm-driver=virtualbox --v=7 --alsologtostderr memory=16384 --cpus=2

/Volumes/MySpace/k8slab took 2m 6s
[I] ➜ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

/Volumes/MySpace/k8slab
[I] ➜ kubectl get nodes
NAME       STATUS   ROLES    AGE    VERSION
minikube   Ready    master   3m2s   v1.19.0

/Volumes/MySpace/k8slab
[I] ➜ kubectl version
Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.0", GitCommit:"e19964183377d0ec2052d1f1fa930c4d7575bd50", GitTreeState:"clean", BuildDate:"2020-08-26T21:54:15Z", GoVersion:"go1.15", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.0", GitCommit:"e19964183377d0ec2052d1f1fa930c4d7575bd50", GitTreeState:"clean", BuildDate:"2020-08-26T14:23:04Z", GoVersion:"go1.15", Compiler:"gc", Platform:"linux/amd /0.1s
```

K8s server 和 client 版本都是 v1.19.0

6. 部署 app

部署一个 hello-minikube 应用，然后打开 K8s 的管理 dashboard：

```bash
/Volumes/MySpace/k8slab
[I] ➜ kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
deployment.apps/hello-minikube created

/Volumes/MySpace/k8slab
[I] ➜ kubectl expose deployment hello-minikube --type=NodePort --port=8080
service/hello-minikube exposed

/Volumes/MySpace/k8slab
[I] ➜ minikube dashboard
🔌  Enabling dashboard ...
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:59659/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
```

7. 安装 Lens

除了 K8s 自带的 dashboard，Lens 是其中一个第三方提供的图形管理工具，[下载安装](https://k8slens.dev/)，非常漂亮：

-   运行，点 ➕，加入 minikube（new）cluster：  
    ![cluster settings](/images/k8s/lens-settings.png)

-   去 Settings，features 里选 metrics：  
    ![cluster metrics](/images/k8s/lens-features.png)

![lens](/images/k8s/lens.png)

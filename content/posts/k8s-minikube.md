+++
title = "Kubernetes 安装 - minikube"
date = 2020-08-30T03:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = false
draft = true
+++

本地安装 Kubernetes，minikube 是最简易的方式

<!--more-->

## 安装 minikube

1. 检查您的 macOS 是否支持虚拟化技术，请运行下面的命令：

```bash
[I] ➜ sysctl -a | grep -E --color 'machdep.cpu.features|VMX'
machdep.cpu.features: FPU VME DE PSE TSC MSR PAE MCE CX8 APIC SEP MTRR PGE MCA CMOV PAT PSE36 CLFSH DS ACPI MMX FXSR SSE SSE2 SS HTT TM PBE SSE3 PCLMULQDQ DTES64 MON DSCPL VMX EST TM2 SSSE3 FMA CX16 TPR PDCM SSE4.1 SSE4.2 x2APIC MOVBE POPCNT AES PCID XSAVE OSXSAVE SEGLIM64 TSCTMR AVX1.0 RDRAND F16C
```

如果你在输出结果中看到了 VMX （应该会高亮显示）的字眼，说明您的电脑已启用 VT-x 特性。

2. 安装 Hypervisor

我选的是 VirtualBox：

```bash
[I] ➜ brew cask install virtualbox
```

3. 安装 kubectl

```bash
[I] ➜ brew install kubectl
```

4. 安装 minikube

```bash
[I] ➜ brew install minikube
```

5. 启动 minikube

```bash
[I] ➜ minikube start --memory=16384 --cpus=4
😄  minikube v1.11.0 on Darwin 10.15.5
✨  Automatically selected the virtualbox driver
👍  Starting control plane node minikube in cluster minikube
🔥  Creating virtualbox VM (CPUs=4, Memory=16384MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.18.3 on Docker 19.03.8 ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube"

[I] ➜ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

[I] ➜ kubectl cluster-info
Kubernetes master is running at https://192.168.99.101:8443
KubeDNS is running at https://192.168.99.101:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

6. 部署 app

```bash
[I] ➜ kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
deployment.apps/hello-minikube created                                                                              /1.1s

~ at ☸️  minikube
[I] ➜ kubectl expose deployment hello-minikube --type=NodePort --port=8080
service/hello-minikube exposed                                                                                      /0.1s

~ at ☸️  minikube
[I] ➜ minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:56851/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
```

## kubectl

[kubectl cheet sheet](https://linuxacademy.com/site-content/uploads/2019/04/Kubernetes-Cheat-Sheet_07182019.pdf)

## 安装 Lens

非常漂亮的 Kubernetes IDE

1. [下载安装](https://k8slens.dev/)

2. 运行，点 ➕，加入 minikube（new）cluster

3. 去 Settings，features 里选 metrics

![cluster settings](/images/k8s/lens-settings.png)
![cluster metrics](/images/k8s/lens-features.png)
![lens](/images/k8s/lens.png)

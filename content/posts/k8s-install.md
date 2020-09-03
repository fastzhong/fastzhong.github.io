+++
title = "Kubernetes 安装方式简介"
date = 2020-08-30T13:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = false
+++

其实 3、4 年前，安装 Kubernetes 还是一个很有挑战性的工作，现在技术越来越成熟了，手段也越来越丰富。

<!--more-->

有几种方式来练手：

-   直接使用别人提供的 sandbox，如 [katacoda](https://katacoda.com/)、[play with k8s](https://training.play-with-kubernetes.com/kubernetes-workshop/) 等
-   直接使用云端 k8s 服务，三大云提供商 - AWS，GPE，Azure 都有
-   云端自建，只用虚机等基础设施
-   本地自建

前两种无需安装，自建方法也有好多，常见的：

-   [Docker Desktop](https://docs.docker.com/docker-for-windows/install/)

    -   如果要使用 Windows 容器，这是唯一选择（并且一定要使用 Hyper-V 虚机）
    -   要求 Windows 10 Pro
    -   倾向采用 Type-1 hypervisor（如 Hyper-V），而非常用的 Type-2（如 VirtualBox），而且两者不兼容（Type1 和 Type2 不可以同时运行）

-   [Minikube](https://k8smeetup.github.io/docs/getting-started-guides/minikube/)

    -   目前最成熟的方案，支持不同平台和版本（Win，Mac，Linux），但需要 vm
    -   配置自由度高
    -   安装和卸载没有其它方案容易
    -   目前只支持单节点的 k8s 集群

*   [kubeadm](https://k8smeetup.github.io/docs/setup/independent/create-cluster-kubeadm/)

    -   安装工具，介于徒手和安装软件之间，可以简化操作
    -   不完全成熟，有些操作还无法无缝完成

-   [Kind（Kubernetes in Docker）](https://kind.sigs.k8s.io/docs/user/quick-start/)

    -   如其名，把整个 k8s 打包在 Docker 里面(本地需要先安装 Docker)
    -   主要用于集成测试（官方用于 k8s 版本发布），未必适合开发者

-   [Microk8s/K3s](https://k3s.io/)

    -   原生 Linux 集成，不需要额外的 vm （所以只支持 Linux）
    -   资源消耗最小，非常轻巧，速度最快，适合 k8s 开发者

*   [Sealos](https://github.com/fanux/sealos)

    -   [使用 Sealos 在 3 分钟内快速部署一个生产级别的 Kubernetes 高可用集群](https://juejin.im/post/5da44a765188251b643eb5e9)

*   [The Hard Way（THW）](https://github.com/kelseyhightower/kubernetes-the-hard-way) - 徒手制造，最早来自 k8s 大名人 kelseyhightower，最灵活也最底层，以生产级别（多节点的）k8s 集群的安装最为复杂

*   借助自动化工具，如 Ansible，Chef，Terraform，等等衍生出来的安装方案，配置和安装的方式自由度高，如 [kubespray](https://github.com/kubernetes-sigs/kubespray)，这里也不展开讨论，尝试的话参考：

    -   [kubespray – 12 Steps for Installing a Production Ready Kubernetes Cluster](https://jhooq.com/kubespray-12-steps-for-installing-a-production-ready-kubernetes-cluster/)
    -   [使用 kubespray 搭建生产级高可用集群](https://www.bboy.app/2020/07/20/%E4%BD%BF%E7%94%A8kubespray%E6%90%AD%E5%BB%BA%E7%94%9F%E4%BA%A7%E7%BA%A7%E9%AB%98%E5%8F%AF%E7%94%A8%E9%9B%86%E7%BE%A4/)

*   k8s 商业软件，如 Openshift，CloudFoundry，Rancher，这些都是以 k8s 为基础的商业集成软件，内含 k8s 的安装和定制方案，非原生，相当重型，面向企业级应用，这里也不展开讨论

各种安装请参考 [Kubernetes Provisioning](https://www.youtube.com/playlist?list=PL34sAs7_26wODP4j6owN-36Vg-KbACgkT)，自建主要是为了学习的，有时间的话可以尝试 THW。
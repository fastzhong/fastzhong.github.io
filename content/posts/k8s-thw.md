+++
title = "Kubernetes The Hard Way"
date = 2019-01-03T13:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = true
draft = true
+++

上篇介绍 minikube 快速安装方式，这篇介绍 The Hard Way（THW）⏤ 徒手方式

<!--more-->

其实 1、2 年前，Kubernetes 的安装还是一个有挑战性的工作，现在技术越来越成熟了。

## 安装方式简介

有几种方式来练手：

-   直接使用别人提供的 sandbox，如
-   直接使用云端 k8s 服务，云提供商基本都有，如 Amazon EKS（Amazon Elastic Kubernetes Service），Azure Kubernetes Service（AKS）等
-   云端自建
-   本地自建

前两种无需安装，自建方法也有好多，常见的：

-   [Docker Desktop](https://docs.docker.com/docker-for-windows/install/)

    -   如果要使用 Windows 容器，这是唯一选择（并且使用 Hyper-V）
    -   要求 Windows 10 Pro
    -   倾向采用 Type-1 hypervisor（如 Hyper-V），而非常用的是 Type-2（如 VirtualBox），和本地 vm 不兼容（Type1 和 Type2 不可以同时运行）

-   [Minikube](https://k8smeetup.github.io/docs/getting-started-guides/minikube/)

    -   目前最成熟的方案，支持不同平台和版本（Win，Mac，Linux），但需要 vm
    -   配置自由度高
    -   安装和卸载没有其它方案容易
    -   目前只支持单节点的 k8s 集群

-   Microk8s

    -   原生 Linux 集成，不需要额外的 vm （只支持 Linux）
    -   非常轻巧，适合本地开发者

-   kind（Kubernetes in Docker）

    -   如其名，把整个 k8s 打包在 Docker 里面
    -   主要用于集成测试（官方用于 k8s 版本发布），未必适合开发者

-   kubeadm

    -   安装工具，介于徒手和安装软件之间，可以简化操作
    -   不完全成熟，有些操作还无法无缝完成

- Sealos 
  - https://github.com/fanux/sealos
  - https://juejin.im/post/5da44a765188251b643eb5e9

-   借助自动化工具，如 Ansible，Chef，Terraform，等等衍生出来的安装方案，配置和安装的方式自由度高，如 [kubespray](https://github.com/kubernetes-sigs/kubespray)，这里也不展开讨论（尝试的话参考：[kubespray – 12 Steps for Installing a Production Ready Kubernetes Cluster](https://jhooq.com/kubespray-12-steps-for-installing-a-production-ready-kubernetes-cluster/)）

-   The Hard Way：完全徒手，不借助任何工具或者 script，完全环保

-   k8s 商业软件，如 Openshift，CloudFoundry，Rancher，这些都是以 k8s 为基础的商业集成软件，内含 k8s 的安装和定制方案，非原生，相当重型，面向企业级应用，这里也不展开讨论

各种安装请参考[Kubernetes Provisioning](https://www.youtube.com/playlist?list=PL34sAs7_26wODP4j6owN-36Vg-KbACgkT)，自建主要是为了学习的，所以决定走一趟 The Hard Way（THW）。THW 项目最早来自 k8s 大名人

## THW

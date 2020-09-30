+++
title = "Kubernetes 实操（Mac）- 目录"
date = 2020-09-17T05:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = false
draft = true
+++

纸上得来终觉浅 绝知此事要躬行

<!--more-->

前提准备：k8s 安装（minikube，kubeadm，或 THW 任何一种都行）  
<i class="fas fa-external-link-alt"></i> &nbsp;&nbsp; [Kubernetes 安装 - minikube](/posts/k8s-install-minikube/)  
<i class="fas fa-external-link-alt"></i> &nbsp;&nbsp; [Kubernetes 安装 - kubeadm](/posts/k8s-install-kubeadm/)  
<i class="fas fa-external-link-alt"></i> &nbsp;&nbsp; [Kubernetes 安装 - THW](/posts/k8s-install-thw/)

接下来的主要步骤：

1. 打包、部署并运行一个 SpringBoot 微服务
2. 日志系统：Loki
3. 跟踪系统：Jager
4. 监控 & 报警：Prometheus + Grafana
5. 混沌工程：Chaos Mesh

在此基础上结合 CI/CD（如 Jenkins）可以构建完整的 5C 流程。

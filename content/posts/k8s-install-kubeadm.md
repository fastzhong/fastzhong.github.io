+++
title = "Kubernetes 安装 - kubeadm"
date = 2020-09-05T03:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = false
draft = true
+++

官方集群推荐工具 kubeadm

<!--more-->

[Bootstrapping clusters with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/)  
[Kubeadm 安装 Kubernetes1.16.2 集群](https://mrbird.cc/Kubeadm-install-Kubernetes1-16-2-cluster.html)  
[使用 kubeadm 快速部署一个 Kubernetes 集群（v1.18）](http://blog.ctnrs.com/post/k8s-kubeadm-install/)  
[用 kubeadm 搭建 k8s 高可用（apt 版）](https://cloudnative365.github.io/keynotes_L4_architect_1_HA_2_k8s_cluster_kubeadm_apt.html)

单节点集群：一个 master 节点；两个 worker 节点

![](/images/kubernetes/single-master.jpg)

| 节点（角色）     | IP                     | 组件                                                              |
| ---------------- | ---------------------- | ----------------------------------------------------------------- |
| k8s\-master1     | 192\.168\.100\.1       | kube\-apiserver，kube\-controller\-manager，kube\-scheduler，etcd |
| ~~k8s\-master2~~ | ~~192\.168\.100\.1~~   | ~~kube\-apiserver，kube\-controller\-manager，kube\-scheduler~~   |
| k8s\-node1       | 192\.168\.100\.11      | kubelet，kube\-proxy，docker，~~etcd~~                            |
| k8s\-node2       | 192\.168\.100\.12      | kubelet，kube\-proxy，docker，~~etcd~~                            |
| ~~k8s\-lb~~      | ~~192\.168\.100\.100~~ | ~~Nginx~~                                                         |

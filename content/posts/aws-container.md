+++
title = "AWS 容器"
date = 1999-08-01T13:08:47+08:00
readingTime = true
categories = ["Cloud Native"]
tags = ["aws"]
toc = true 
draft = true
+++

ECS，EKS，Fargate，等

<!--more-->

https://kublr.com/blog/comparing-aws-ecs-and-self-managed-kubernetes-ecs-tutorial/

https://www.justaftermidnight247.com/insights/eks-vs-ecs-vs-fargate-vs-kubernetes-aws-containers-explained/
https://cloudcomputing-news.net/news/2021/may/13/aws-eks-vs-ecs-vs-fargate-which-offers-the-best-managed-kubernetes/
https://cast.ai/blog/aws-eks-vs-ecs-vs-fargate-where-to-manage-your-kubernetes/

ECR

ECS EKS K8s

Fargate

Fargate on-demand
Fargate Spot
SIGTERM
SERVICE_TASK_PLACEMENT_FAILURE

Deployment

https://www.stackrox.com/post/2021/01/eks-vs-gke-vs-aks-jan2021/

基本信息
AKS 提供了更新的 Kubernetes 版本支持，通常比 GKE 和 EKS 新两到三个版本。
AKS 和 GKE 都提供了控制平面的自动更新机制（AKS 还在预览版），而 EKS 不支持自动更新。
AKS 和 GKE 都提供了 Node 健康检测和自动恢复机制，而 EKS 需要用户自己负责。
GKE 和 EKS 的控制平面都是收费的，而 AKS 还是继续免费。
在运行时上，三大平台都同时支持 Docker 和 containerd，所以上游社区 Docker 弃用不影响这些平台。除此之外，GKE 还支持独有的 gVisor，适用于安全要求更高的场景。
综合这些基本信息，AKS 和 GKE 相对于 EKS 来说，托管服务提供了更多的自动诊断和自动恢复机制，具有明显的优势。而 AKS 的控制平面还是免费的，这对小型客户来说，具有不小的吸引力。

服务限制
在集群节点数量上，GKE 最多支持 15000 个节点，EKS 支持 3000 个节点，而 AKS 暂时只支持 1000 个节点。
在节点池数量上，AKS 支持 100 个节点池，EKS 支持 100，而 GKE 没有明确的文档。
在每个节点池支持的节点数量上，AKS 和 EKS 都支持 100，而 GKE 支持 1000。
在每个节点支持的 Pod 数量上，AKS 支持 250，GKE 支持 110， 而 EKS 需要用户根据具体网络配置计算。
综合这些服务限制，GKE 单集群支持更多的节点数，超过 5000 节点时，GKE 是唯一的选择。

网络和安全
在网络和安全上，三大平台都默认开启了 RBAC，都支持 PodSecurityPolicy。AKS 还支持 Azure Policy，在涵盖 PodSecurityPolicy 基础上，还支持配置 Azure 平台相关的策略。
在 Kubernetes API 的访问限制上，三大平台都提供了白名单机制和私有 API 地址的功能。
在网络策略（Network Policy）的支持上，EKS 需要用户手动去安装 Calico，而 AKS 和 GKE 都支持内置开启。
虽然 EKS 提供了托管节点池的功能，但这个功能要求每个节点都绑定一个公网 IP，这对网络安全来说是个很大的挑战。
综合这些网络和安全特性，AKS 和 GKE 提供了更完善的安全控制机制，而 EKS 还有很多配置需要用户自己管理。

镜像服务
在镜像服务上，三大平台都提供了镜像仓库服务，支持匿名或私有的镜像托管。ECR 和 ACR 都同时支持 Docker 镜像格式、OCI 镜像格式以及 Helm Chart，而 GKE 已经从大家熟知的 GCR 迁移到了 Artifact Registry （AR），支持 Docker 镜像格式、OCI 镜像格式以及 Maven 和 npm 等。
在镜像安全上，三大平台都提供了镜像安全扫描服务。
在镜像可用性上，三大平台都提供了跨地域冗余的机制，自动把镜像数据复制到其他地域。
综合这些镜像服务的特性，可以发现三个平台提供的镜像服务基本类似，AKS 和 EKS 支持托管 Helm Charts，是相对于 GKE 的优势。

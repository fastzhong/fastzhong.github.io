+++
title = "DevOps  概揽"
date = "2018-08-15T12:19:19+08:00"
categories = ["DevOps"]
tags = []
toc = false
+++

别扯了，DevOps 的核心就是自动化 - `全程自动化`

<!--more-->

很多人谈论 DevOps/CICD，其实是这 5 个基本点：

-   Continuous Integration
-   Continuous Testing
-   Continuous Delivery
-   Continuous Deployment
-   Continuous Monitoring

这 5 个 C 的支撑点就是自动化工具：

![cicd](/images/devops/devops-tools.png#center)

[CNCF Cloud Native Landscape](https://landscape.cncf.io/) 展现了大量容器相关的工具。

DevOps/CICD 做得好挺难的，因为一方面软件运行环境多样化和复杂化，新技术层出不穷，另一方面自动化工具的开发需要时间，而且众口难调。

而我理解最好的 DevOps 则是自动化能够做到覆盖软件生命的全周期，并且 **_全程融合_**（支持上下文承接），_**全程反馈**_（如同分布式监控可对各个子系统全程监控，包括人为反馈），已达至 **_全程自动化_**，而不是孤立的使用各个工具。这里有两个核心概念，一个是自动化，一个是全程，具体体现在（一些老大难的地方）：

-   基础设施全自动化

-   安全管理全自动化

-   数据库运维全自动化

-   测试全自动化

-   大规模监控、报警、自恢复

之前为一个微服务项目构建的 pipeline，基本上挺完整的：

![微服务 Devops Pipeline](/images/devops/devops-pipeline.jpg#center)

准备以此打造一个满足中小型团队的现代化流水线原型。

[Production Readiness Checklist](https://www.gruntwork.io/devops-checklist/)

![Production Readiness Checklist](/images/devops/checklist.png#center)

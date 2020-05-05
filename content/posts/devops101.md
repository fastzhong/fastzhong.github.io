+++
title = "DevOps  概揽"
date = "2018-08-15T12:19:19+08:00"
categories = ["DevOps"]
tags = []
toc = false
+++

别扯了，DevOps 的核心就是自动化 - `全程自动化`

<!--more-->

很多人谈论 DevOps，其实是这 5 个基本点：

-   Continuous Integration
-   Continuous Testing
-   Continuous Delivery
-   Continuous Deployment
-   Continuous Monitoring

这 5 个 C 的支撑点就是自动化工具，而我理解最好的 DevOps 则是自动化能够覆盖软件生命的全周期，并且做到 **_全程融合_**（支持上下文承接），_**全程反馈**_（如同分布式监控可对各个子系统全程监控，包括人为反馈），已达至 **_全程自动化_**。

![cicd](/images/devops/cicd.png)

各阶段核心系统：

-   _需求阶段_ ⏤ <font color="yellow">Jira</font>  
    需求有多种，不只是直接来自用户或项目的需求，还包括设计实施需求，测试需求，发布需求，硬件/密码变更需求，bug 管理需求，等等。大需求可以拆分出小需求，或引出不同种类的需求。Jira 的流水单号，就像分布式监控的 trace no，所有的动作必须是基于一个流水单号开始的。另外复杂系统本身还存在多对一的情况，例如一次软件发布对应多个需求，这时需要给每个需求打上不同的标签，如所属性质标签，子系统标签，发布标签，团队标签，sprint 标签，等等，通过标签把相关的流水单汇总。需求管理是整个 DevOps 的起点和中心。

*   _开发阶段_ ⏤ <font color="yellow">IDE</font>

    -   软件变更管理：主要就是使用 Git，每个 commit 必须对应需求的流水单号
    -   代码团队协作：这里可以采用各种 Git workflow 的分支管理
    -   本地开发、测试支持

-   _构建，集成测试阶段_ ⏤ <font color="yellow">Jenkins</font>

    -   各种构建、测试环境和测试案例
    -   软件制品：这里包含各种打包工具，还有代码质量扫描，安全扫描，软件制品仓库，等等。软件制品除了二进制本身外，需要包含软件的一系列元信息，例如发布标签，部署标签，等等。

-   _打包，发布阶段_ ⏤ <font color="yellow">Artifactory，发布系统</font>  
    由于基础实施的不同，软件的不同，系统性质的不同，发布流程和管理是比较复杂和多样化的，如回滚，蓝绿部署，金丝雀部署。这也是现代 DevOps 的难点之一，以及和传统运维的区别。

-   _运维阶段_ ⏤ <font color="yellow">Kubernetes，监控系统</font>

[CNCF Cloud Native Landscape](https://landscape.cncf.io/) 大量的工具支撑：

![cncf](/images/devops/cncf.png)

`全程自动化` 是运维的目标，这里有两个核心概念，一个是自动化，一个是全程，具体体现在（一些老大难的地方）：

-   基础设施全自动化

-   数据库运维全自动化

-   安全管理全自动化

-   监控、报警、自恢复全自动化

之前为一个微服务项目构建的 pipeline，基本上挺完整的：

![微服务 Devops Pipeline](/images/ms-devops-pipeline.jpg)

🗓TBD：目前我自己为团队设计和打造的流水线（基于 AWS）

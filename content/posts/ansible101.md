+++
title = "Ansible 快速上手"
date = 2010-08-01T13:08:47+08:00
readingTime = true
categories = ["DevOps"]
tags = ["ansible"]
toc = true 
draft = true
+++

Ansible，运维工具里的瑞士军刀，比老牌 Puppet，Chef 好用太多

<!--more-->

💬 很多人会告诉你自动化或者工具（automation）不是 DevOps，但他们没有告诉你什么是 DevOps 的核心，如果你把自动化（automation）从他们的表述中除去，你看看 DevOps 还是什么呢。Ansible 从核心上讲，就是一个 automation 的框架，通过各种扩展，完成自动化的各式各样任务，如创建用户，安装软件，等。


# Why Ansible

做 IT 的的都知道，除了设计，写代码，离不开装机器，装软件，装系统，原始的办法是手动安装和配置，但对于一个大型项目、产品或系统:

-   需要装很多次(1000+)
-   需要在很多机器上装（1000+）
-   需要在不同的环境和不同的OS上装
-   需要为不同的客户定制安装 
-   如果是手工行为，步骤超多，如何防止人为疏忽
-   安装、运维文档如何更新，维护

例如银行的每个子系统，十几年一直在不停的开发，安装，部署，测试，上线。

> 种一颗树和种 1000 颗树的方法是不同的

最原始的解决办法就是写脚本，尽量自动化，让机器来替我们干活 - 机器不累，不出错。但如何写出高质量，可复用，可叠加的脚本呢？这就是为什么需要 Ansible，它的核心完成两个最基本的事情：

-   **配置管理**（configuration or change management）：Ansible 不是专门的配置管理系统 CMDB（configuration management database），但没有这些配置信息无法干活啦，一般软件项目需要的基础配置也就是 ip, hostname, config file, 这些东东足已。Ansbile 可以和 CMDB 结合工作，达成动态配置，这个“高级”用法就不在本文讨论范围。

-   **自动化框架**（automation framework）：注意不只是 automation，Ansible 是在Python上实现的，把自动化的动作按可编程的方式编写和组织，按可编程方式调用和共享，这样才能完成其它的复杂事情如编排（orchestration：把几个事情串在一起执行），这就比脚本和脚本库高一个档次。使用者也不必纠缠于命令行的参数，选项等等各种细节。Ansbile 使用上采用类似 SQL“声明式”的理念，而不是一大段具体的实现细节，关注点在“What”上而不是“How”。


run countless time with consistent result

DB installation

why easy

1. yml

2. ssh instead of agent

3. infra as code

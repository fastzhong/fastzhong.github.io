+++
title = "Ansible 快速上手到项目最佳实战"
date = 2017-02-28T13:08:47+08:00
readingTime = true
categories = ["DevOps"]
tags = ["ansible"]
toc = true 
+++

Ansible，运维工具里的瑞士军刀，比老牌 Puppet，Chef 好用太多

<!--more-->

💬 很多人会告诉你自动化工具（automation）不是 DevOps，但他们没有告诉你什么是 DevOps 的核心，如果你把自动化（automation）从他们的表述中除去，你看看 DevOps 还是什么呢。Ansible 从核心上讲，就是一个可编程的 automation 框架，通过各种扩展，完成自动化的各式各样任务，如创建用户，安装软件，等。

# Why Ansible

做 IT 的的都知道，除了设计，写代码，离不开装机器，装软件，装系统，原始的办法是手动安装和配置，但对于一个大型项目、产品或系统:

-   需要装很多次(1000+)
-   需要在很多机器上装（1000+）
-   需要在不同的环境和不同的 OS 上装
-   需要为不同的客户定制安装
-   如果是手工行为，步骤超多，如何防止人为疏忽
-   安装、运维文档如何更新，维护

> 种一颗树和种 1000 颗树的方法是不同的

最原始的解决办法就是写脚本，尽量自动化，让机器来替我们干活 - 机器不累，不出错。但如何写出高质量，可复用，可叠加的脚本呢 ⏤ 脚本就比较难胜任了。这就是为什么需要 Ansible，它的核心完成两个最基本的事情：

-   **配置管理**（configuration or change management）：Ansible 不是专门的配置管理系统 CMDB（configuration management database），但没有这些配置信息无法干活啦，一般软件项目需要的基础配置也就是 ip, hostname, 环境变量，config file, password，等等。Ansible 能够提供配置管理，但更像是 CMDB 的执行者，把配置实现到机器上。Ansible 可以和 各种 CMDB 实时结合工作，达成配置动态收集，动态更改，动态实现，这个“高级”用法就不在本文讨论范围。

-   **自动化框架**（automation framework）：注意不只是 automation，这里其实包含了几重意思
    -   Ansible 是用 Python 实现的（Linux shell 的大部分命令已用 Python 实现），可以干几乎任何编程语言能干的事情，所以是可编程的自动化框架，实现自动化的扩展和重用，Python 是一种超级”胶水“语言，贴近自然语言，运维二次开发非常容易；
    -   ssh instead of agent：ssh，Python 在 Linux 几乎所有的版本上自带，所以 Ansible 的使用依赖几乎为 0（唯一问题是诞生之初不考虑 Windows，最近才支持）
    -   Ansible 没有像 Chef、Puppet 那样复杂，二次，三次，四次抽象的概念和架构，有运维知识，1，2 小时基本就可上手，Ansible 贴近 shell/Linux 原生系统，一条 Ansible 命令影响成百上千台机器，任何复杂系统的配置包装一下就可以融入 Ansible，简单粗暴，但非常灵活，对于运维这种千奇百怪的脏活、累活，超级耐操；
    -   Ansible 使用上采用类似 SQL“声明式”的 命令（yaml），而不是一大段具体的自动化动作细节，关注点在“What”上而不是“How”，这是自动化层面的抽象，有别于脚本，使用者也不必纠缠于五花八门的命令行用法细节；
    -   编排自动化的动作（orchestration：把几个事情串在一起执行像交响乐演奏需要各种乐器的一起配合），这样才能完成用户特定的复杂事情，这就比脚本和脚本库高一个档次；

在 IT 界，重复发明轮子的事多了去，Ansible 也属于此，但和老牌的 Chef、Puppet 相比，优势在于易用、好用、耐用。

## 工作原理和基本概念

## 基本操作

## 项目实战

### 项目模版

💡 快速实战必读：[Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)

```bash
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
   group1.yml             # here we assign variables to particular groups
   group2.yml
host_vars/
   hostname1.yml          # here we assign variables to particular systems
   hostname2.yml

library/                  # if any custom modules, put them here (optional)
module_utils/             # if any custom module_utils to support modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier

roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```

当然此模版有瑕疵，重点如下：

### 重点

### 四部曲

## 技巧

run countless time with consistent result

DB installation

why easy

1. yml

2. ssh instead of agent

3. infra as code

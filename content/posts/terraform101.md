+++
title = "Terraform 快速上手到项目最佳实战"
date = 2021-05-10T13:08:47+08:00
readingTime = true
categories = ["DevOps"]
tags = ["terraform"]
toc = true 
+++

Terraform， Infrastructure as Code（IaC）的必备工具，特别是云时代

<!--more-->

为什么 IaC（可编程，可版本化，可测试化，减少人为失误，多人合作，等等）？IaC 实质是一切代码化，从而体现软件的价值 - 可重用和可扩展，Terraform 是个 infra 自动化工具，但专职于 Infrastructure setup/provisioning，之后的事可以交给 Ansible，参考： <i class="fas fa-external-link-alt"></i>&nbsp; [Ansible 快速上手到项目最佳实战](/posts/ansible101/) 。

https://www.terraform.io/docs/cloud/guides/recommended-practices/index.html

How to test IaC？

通过测试我们对应用软件的质量比较有信心，同样 IaC 我们也可以采用同样的测试手段：

1. static analysis（compiler，linter， dry-run）
2. unit tests

unit tests 拆分成不同的 module

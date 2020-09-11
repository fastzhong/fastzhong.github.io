+++
title = "CKA & CKAD 2020"
date = 2020-09-09T06:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = false
draft = true
+++

拖了几年，打算在 2020 年末拿下

<!--more-->

## CKA & CKAD 介绍

考试收费为 300 美元/人，官方报名网站：

-   [CKA](https://www.cncf.io/certification/CKA/)
-   [CKAD](https://www.cncf.io/certification/CKAD/)

官方的参考信息：

-   [Candidate Handbook](https://training.linuxfoundation.org/go/cka-ckad-candidate-handbook)
-   [Curriculum Overview](https://github.com/cncf/curriculum)
-   [Exam Tips](http://training.linuxfoundation.org/go//Important-Tips-CKA-CKAD)
-   [Frequently Asked Questions](http://training.linuxfoundation.org/go/cka-ckad-faq)

✦ 考试预约  
购买考试后，考试日期需要预约（Schedule Exam），预约成功后，考生将会收到一封预约确认邮件，若需取消或重新安排预约时间，需在开考前 24 小时前进行操作。

✦ Important Tips (重要提醒)
考生可下载并阅读 My Portal 页面的 Important Tips。

✦ 检查硬件兼容性  
考生必须检查其系统是否符合考试最低要求：
点击 Exam Prep Checklist 中的“Check System Requirements”链接；  
进入“Compatibility Check Tool”（兼容性检查工具）页面；  
或直接访问：https://www.examslocal.com/ScheduleExam/Home/CompatibilityCheck  
在 Exam Sponsor 项下选择“Linux Foundation”，在 Exam 项下选择“CKA”；

✦ 考试现场环境  
需要一个安静的房间：任何公共场合（如咖啡厅等）均不能作为考场，同时环境内尽量简洁，考试前需要用摄像头四周进行转动。同时考试是可以使用透明的杯子喝水的（建议放在旁边）。

✦ 考试方式  
软件版本 Kubernetes `v1.19`，考试时长 2 小时，总共 19 道题，总分 100 分，66 分通过。只使用 Chrome 浏览器，不会使用到其他软件，通过浏览器调用摄像头，麦克风，桌面分享等功能。可接受一次重考。

✦ 考官交流  
与对方聊天全程于右下角窗口打字，对方全程监控，并下达各种指令。对方会要求我们分享摄像头，分享桌面，没事不要乱动。全英文交流，所以只要英文阅读能力不错就行，其他时间可以全程输入命令操作即可。

✦ 操作方式  
考试为线上实操考试，所有操作均在 Chrmoe 浏览器完成，所有命令均在终端操作完成，同时该终端操作只能连接一个服务端，无标签功能。

💡 需要注意的地方：

-   考试环境是浏览器里的 terminal，其实就是命令行，所以必须熟悉 Linux，特别是 Ubuntu
-   命令行的编辑器是 vi/vim，所以也必须熟悉

关于 Linux，Vi/Vim 网上的教程很多，关键还是有动手练习至熟练。

## tips

考试 tips：

```cmd
- 考试过程中即使出现中断，考试网页右上角显示offline，这时候不要关闭网页，而是直接F5刷新考试界面，等于重新连接。
- 重新连接或者异常退出，考试的环境的所有操作均会保留，因为考试的所有命令其实等于是通过Chrome浏览器远程敲在另一端的主机上的。
- 考试的时候，考官提示 ctrl+c 和 ctrl+v 不能用，而且还提示只能复制粘贴少量的行，但实测其实考试操作的网页上不能用，但是在参考的 Kubernetes.io 中完全可以用，而且可以整段的复制，同时还可以使用鼠标右键复制。
- 所有题目中的关键字，都会以深颜色标注，如namespace名字，pod名字等，最方便的是可以直接鼠标左键点击就是复制。
- 考试存在四套集群环境，所有题目均会给出相应的上下文切换命令用于切换集群，需要的话，不要忘记切换，个别题目需要切换集群和ssh至其他集群，一定要先切换集群然后ssh，而不能先ssh再切换集群。
```

准备 tips：

```cmd
-   熟悉 Kubernetes 文档，考试时官方允许参考的网页只有三个：https://kubernetes.io/docs/ 及其子域、https://github.com/kubernetes/ 及其子域或https://kubernetes.io/blog/ 。
```

## 准备课程

我看过的课程：

㊫ [深入剖析 Kubernetes](https://time.geekbang.org/column/intro/116)  
㊫ [阿里云与 CNCF 联合推出的云原生技术公开课](https://edu.aliyun.com/roadmap/cloudnative)  
㊫ [华为 CloudNativeLives 之深入剖析 Kubernetes 系列课](https://bbs.huaweicloud.com/forum/thread-17415-1-1.html)  
㊫ [华为 CloudNativeLives 之 Kubernetes 管理员实训系列课](https://bbs.huaweicloud.com/forum/thread-17416-1-1.html)

特别推荐张磊大牛的两个课程 👉 一个是极客时间，另一个是阿里云，可能不是准备考试或学习具体 Kubernetes 技术最好的教程，但很好兼顾了技术的宽带和深度，对了解 Kubernetes/云原生的来龙去脉大有裨益。

## 练习

由于 CKA/CKAD 都是动手实操，所以练习很重要！！！

CKA 操练题：

CKAD 操练题：

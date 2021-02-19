+++
title = "CKA & CKAD 2020"
date = 2002-10-30T06:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["kubernetes"]
toc = true
draft = true
+++

拖了好几年，打算在 2020 年末拿下

<!--more-->

## CKA & CKAD 介绍

考试收费为 300 美元/人，官方报名网站：

> -   [CKA](https://www.cncf.io/certification/CKA/)
> -   [CKAD](https://www.cncf.io/certification/CKAD/)

官方的参考信息：

> -   [Candidate Handbook](https://training.linuxfoundation.org/go/cka-ckad-candidate-handbook)
> -   [Exam Tips](http://training.linuxfoundation.org/go//Important-Tips-CKA-CKAD)
> -   [Frequently Asked Questions](http://training.linuxfoundation.org/go/cka-ckad-faq)
> -   [Curriculum Overview](https://github.com/cncf/curriculum)

✦ <span style="color: orange">考试预约</span>  
购买考试后，考试日期需要预约（Schedule Exam），预约成功后，考生将会收到一封预约确认邮件，若需取消或重新安排预约时间，需在开考前 24 小时前进行操作。

✦ <span style="color: orange">Important Tips (重要提醒)</span>  
考生可下载并阅读 My Portal 页面的 Important Tips。

✦ <span style="color: orange">检查硬件兼容性</span>  
考生必须检查其系统是否符合考试最低要求：
点击 Exam Prep Checklist 中的“Check System Requirements”链接；  
进入“Compatibility Check Tool”（兼容性检查工具）页面；  
或直接访问：https://www.examslocal.com/ScheduleExam/Home/CompatibilityCheck  
在 Exam Sponsor 项下选择“Linux Foundation”，在 Exam 项下选择“CKA”；

✦ <span style="color: orange">考试形式</span>  
软件版本 Kubernetes `v1.19`；  
CKA 考试时长 2 小时，总共 15 - 20 道题/任务，66% 通过，可接受一次重考；  
CKAD 考试时长 2 小时，总共 17 道题/任务，66 通过，可接受一次重考；  
考试是可以使用透明的杯子喝水的（建议放在旁边）；  
只使用 Chrome 浏览器，不会使用到其他软件，通过浏览器调用摄像头，麦克风，桌面分享等功能；  
Chrome 只能开两个 tab：一个考试，一个 Kubernetes docs 网站；

考官：  
 考试可以提前 15 分钟进入考试界面；  
 与对方聊天全程于右下角窗口打字，全程监控，并下达各种指令，全程英文消息交流；  
 考官会要求我们分享摄像头，分享桌面，没事不要乱动；  
 考官会让你出示能确认你身份 ID 的证件；  
 考官会让你用摄像头环视房间一周，确认你的考试环境；  
 考官会让你用摄像头看你的整个桌面和桌子底下；

## tips

💡 考试 tips：

```cmd
- 考试过程中即使出现中断，考试网页右上角显示offline，这时候不要关闭网页，而是直接F5刷新考试界面，等于重新连接。
- 重新连接或者异常退出，考试的环境的所有操作均会保留，因为考试的所有命令其实等于是通过Chrome浏览器远程敲在另一端的主机上的。
- 考试的时候，考官提示 ctrl+c 和 ctrl+v 不能用，而且还提示只能复制粘贴少量的行，但实测其实考试操作的网页上不能用，但是在参考的 Kubernetes.io 中完全可以用，而且可以整段的复制，同时还可以使用鼠标右键复制。
- 所有题目中的关键字，都会以深颜色标注，如namespace名字，pod名字等，最方便的是可以直接鼠标左键点击就是复制。
- 考试存在四套集群环境，所有题目均会给出相应的上下文切换命令用于切换集群，需要的话，不要忘记切换，个别题目需要切换集群和ssh至其他集群，一定要先切换集群然后ssh，而不能先ssh再切换集群。
```

💡 准备 tips：

```cmd
-   考试环境是浏览器里的 terminal，其实就是命令行，所以必须熟悉 Linux/Ubuntu
-   命令行的编辑器是 vi/vim，所以也必须熟悉基本编辑操作
-   必须非常熟悉 kubectl
-   熟悉 Kubernetes 文档（concept + task），考试时经常查找，考试时官方允许参考的网页只有三个：https://kubernetes.io/docs/ 及其子域、https://github.com/kubernetes/ 及其子域或 https://kubernetes.io/blog/
-   练习 + 练习 + 练习
```

我的 bookmark

💡 命令行 tips：


```bash
sudo -i

```

💡 Vim Tips：

💡 kubeadm tips：

💡 kubectl tips：

## 课程

付费和免费的课程都很多，各大厂都推出了 🆓 课程（Google，Microsoft，IBM，DigitalOcean，等等） [linuxfoundation](https://training.linuxfoundation.org/) 课程去年圣诞有大促销并且免掉了考试费。

我看过的一些课程：

㊫ [深入剖析 Kubernetes](https://time.geekbang.org/column/intro/116)  
㊫ [阿里云与 CNCF 联合推出的云原生技术公开课](https://edu.aliyun.com/roadmap/cloudnative)  
㊫ [华为 CloudNativeLives 之深入剖析 Kubernetes 系列课](https://bbs.huaweicloud.com/forum/thread-17415-1-1.html)  
㊫ [华为 CloudNativeLives 之 Kubernetes 管理员实训系列课](https://bbs.huaweicloud.com/forum/thread-17416-1-1.html)

特别推荐 👉 阿里张磊大牛的课程，可能不是准备考试或学习具体 Kubernetes 技术最好的教程，但对了解 Kubernetes/云原生的来龙去脉大有裨益。

📌 [The Kubernetes Learning Resources List - Google Sheets](https://goo.gl/Rywkpd)

## 练习

🔥 由于 CKA/CKAD 都是实操，所以动手很重要 🔥

### 练习环境

免费环境（[free-kubernetes](https://github.com/learnk8s/free-kubernetes/)）或本地安装（[Kubernetes 安装方式简介](/posts/k8s-install/)）

### CKA 练习

### CKAD 练习

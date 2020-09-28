+++
title = "Docker/容器 101（2020.9更新）"
date = 2017-05-01T07:28:59+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["docker"]
toc = true
+++

Docker 🐳 是继 Java 十多年之后又一个“颠覆性”的技术，接触 Docker 三年有余，过去一年多给队伍做过多次介绍。

<!--more-->

## 问题 & 缘起

做项目或产品的程序猿们肯定没少见过下面的这种文档，从更改记录中可以看到完成一个文档的艰辛。更要命的是一个文档通常只对应一个版本，一个平台，一种环境，一种配置。

![problem](/images/docker/TheProblem1.jpg#center)

![problem](/images/docker/TheProblem2.jpg#center)

在现代化和大规模的软件作业里，文档的问题是显而易见的，维护文档更是苦不堪言。特别是做大型软件产品：

![problem](/images/docker/TheProblem3.jpg#center)

文档的问题其实反应了背后的工程问题 - 软件的安装和配置，还好有自动化运维工具（[Ansible 快速上手到项目最佳实战](/posts/ansible101/)）：

![ansible](/images/docker/ansible.jpg#center)

但如何应对现代软件开发中多环境的问题：

![challenges](/images/docker/challenges.jpg#center)

所以这些工具并没有从根本上解决软件的安装和配置的复杂性，只不过将过程自动化而已，而容器技术从底层将之改观。

## 容器发展史 & 背景

由于容器和很多技术相关，所以先理清一下：

<div align=center>  <!-- 可选的项：right，left，center -->
	<img src="/images/docker/container-history.png" alt="容器历史">  <!-- src处填写路径（本地或网络） width 和 height 就是控制图片的大小的-->
</div>

-   <font color="yellow">Chroot Jail</font>  
    Chroot Jail 应该是第一种容器化技术，90 年代的系统管理员一定对 chroot 不陌生，为了安全，Apache Web 服务器都进行 chroot 配置。

    ![chroot](/images/docker/chroot.gif#center)

-   <font color="yellow">Linux</font>  
    1991 年 Linus Torvalds 在 PC 上开发了 Linux 内核。

-   <font color="yellow">FreeBSD Jail</font>  
    FreeBSD OS 第一次将 chroot 引入操作系统，实现不单是文件而且是进程级别的隔离。

-   <font color="yellow">Linux VServer</font>  
    比 chroot 进一步，Linux VServer 在系统级别实现虚拟化，同一个内核，却可以运行多个不同的 Linux distributions。

-   <font color="yellow">Solaris Container</font>  
    实现 Solaris 版本的 Liunx VServer

-   <font color="yellow">OpenVZ</font>  
    类似 Liunx VServer 和 Solaris Container，但 Linux VServer 和 OpenVZ 都需要对 kernel 打补丁才能支持容器的创建。

-   <font color="yellow">CGroups</font>  
    Google 开发的技术，实现对进程进行资源的控制（CPU、内存、磁盘 I/O、网络，等），CGroups 进入 Linux Kernel。

-   <font color="yellow">LXC</font>  
    和之前的 Liunx VServer、Solaris Container、OpenVZ 类似，但 LXC （<font color="yellow">L</font>inu<font color="yellow">X</font> <font color="yellow">C</font>ontainer）包装了内核原生的 CGroup ，通过一系列的 API 允许普通程序创建和管理容器，每一个容器进程拥有自己的虚拟空间（CPU，内存，I/O，网络，等），实现操作系统层次上的资源的虚拟化。CloudFoundry 在 2013 年开发了 Warden，采用 LXC 并提供 API 来管理动态的容器资源。

    ![lxc](/images/docker/lxc.png#center)

-   <font color="yellow">Apache Mesos</font>  
    2009 年 UC Berkeley RAD 实验室开发的分布式系统运行平台。

-   <font color="yellow">Docker</font>  
    2013 年，基于 LXC 的 Docker 出世：

    ![docker](/images/docker/docker.png#center)

-   <font color="yellow">LMCTFY</font>  
    Google 开源了自己的容器运行技术栈 LMCTFY（Let me contain that for you），同时和 Docker 合作，把其相关的概念和抽象移植到 libcontainer。

-   <font color="yellow">rkt</font>  
    CoreOS 发布和 Docker 类似的 Rocket。

-   <font color="yellow">Kubernetes</font>  
    Google 开源生产级别的容器集群运维管理平台（脱胎于 Google 内部 Borg 系统）。

Jail，Virtual Private Servers，Zones，Containers，VMs，等都是不同的技术，但又有两个共同点，都是为了：  
✅ 资源隔离  
✅ 资源控制

<font color="yellow">VM（Virtual Machine）</font>一般指在实体机器上创建的虚拟机器，VM 又分为 “System Virtual Machine” 和 “Process Virtual Machine”。对于 Guest OS 而言，VM 就像是一台真实的机器，而 Hypervisor 是关键技术，用来处理 VM 的 CPU，内存，网络等，Hypervisor 又分为 Type1 & 2：

![hyper-v](/images/docker/hyper-v.png#center)

```md
-   Type1: VMware vSphere, KVM, Microsoft Hyper-V
-   Type2: Oracle VM VirtualBox, VMware Workstation Pro/VMware Fusion
```

“Process Virtual Machine”又称为“Application Virtual Machine”，如我们通常熟知的 JVM。

有了大致的背景，接下来要解析容器的核心技术，了解后也很容易知道容器“感觉”好像是 VM，其实和 VM 是完全不同的。

## 容器实现原理

要彻底理解容器，要先了解以下 Linux 内核技术：

```md
-   Linux Kernel & User Space （系统空间和用户空间）
-   System Calls （系统调用）
-   Namespaces
-   Cgroups
-   rootfs
```

### Kernel Space & System Calls

![kernel space](/images/docker/kernel-space.png#center)

Linux 把内存空间划分为 Kernel & User Space （系统空间和用户空间），我们的程序都是放在用户空间里，但所有的计算，储存，通讯最终都是通过调用底层的 Kernel 来完成的，而用户和系统空间的交互需要通过一系列系统调用『System Call』。划分的一个重要目的就是对 资源进行保护：

![rings](/images/docker/rings.png#center)

下面是一个 C 的例子给文件申请内存空间，它必须通过系统调用来完成

```c
tmp_buf = mmap(file, len); # mmap here is from a C library
```

调用过程如下：

<div align=center>  <!-- 可选的项：right，left，center -->
	<img src="/images/docker/mmap.jpg" alt="mmap">  <!-- src处填写路径（本地或网络） width 和 height 就是控制图片的大小的-->
</div>

一个程序运行的大致流程是，父进程通过系统调用 让内核产生新的进程，内核保留父程序的上下文，准备初始化子进程的上下文，切换至子进程，操作权这时交到了子进程，子进程的逻辑开始真正运行，逻辑运行结束后，会向父进程发信号，内核回收子进程资源，操作权回到父进程，父进程继续执行接下去的逻辑。举个例子，当在 bash 里运行 ls 的时候，bash 的 process id 假设为 10，该 process 通过 fork()产生出一个新的 process，id 为 11，process 11 通过 execve()把当前的程序/bin/bash unload，接着 load 入/bin/ls，当 ls 运行结束后，process 11 通过系统调用 exit()通知 process 11 的结束状态（status code），内核这时通过 wait()唤醒终止运行的 process 10，process 10 收到 process 11 的 status code 并继续运行：

![process](/images/docker/process.jpg#center)

Linux 有几个特殊的进程，pid 为 0 的 idle 进程被成为上帝进程，其创建 pid 为 1 的 /sbin/init 进程和 pid 为 2 的 kthreadd 进程，前者负责执行内核的一部分初始化工作和系统配置，也会创建一些类似 getty 的注册进程，而后者负责管理和调度其他的内核进程：

![process 0，1，2](/images/docker/process0.png#center)

### namespaces

Linux 内核通过『namespace』提供了资源隔离的功能，各种 namespace 对应于各种资源的抽象数据结构（共 7 种），内核通过这种结构来管理资源，有了 namespace，相当于代码的 package name，每个 process 有自己的资源视角，资源的使用可以单独定制。Linux 目前支持七种资源的 namespace：

| 名称    | 定义            | 隔离的资源                                                                                         |
| ------- | --------------- | -------------------------------------------------------------------------------------------------- |
| IPC     | CLONE_NEWIPC    | System V IPC, POSIX message queues                                                                 |
| Network | CLONE_NEWNET    | network device interfaces, IPv4 and IPv6 protocol stacks, IP routing tables, firewall rules, etc\. |
| Mount   | CLONE_NEWNS     | Mount points                                                                                       |
| PID     | CLONE_NEWPID    | Process IDs                                                                                        |
| User    | CLONE_NEWUSER   | User and group IDs                                                                                 |
| UTS     | CLONE_NEWUTS    | Hostname and NIS domain name                                                                       |
| CGroup  | CLONE_NEWCGROUP | Cgroup root directory                                                                              |

当我们运行一个 Docker 容器时，就是通过 clone 系统调用产生一个带 namespace 的进程（参数 flags 表示使用哪些 CLONE\_\* 标志位）：

```c
// System call
int clone(int (*child_func)(void *), void *child_stack, int flags, void *arg);

// Docker source code to create a container process
int pid = clone(main_function, stack_size, CLONE_NEWPID | SIGCHLD, NULL);
```

![docker run 1](/images/docker/docker-space1.png#center)

内核可以通过 namespace 隔离不同的进程：

![docker run 2](/images/docker/docker-space2.png#center)

我们可以查找进程 pid=100 对应的 namespace，例如：

```bash
namespace git:(uts-demo) ✗ ls -l /proc/100/ns/
total 0
lrwxrwxrwx 1 cizixs cizixs 0 12月 21 15:36 cgroup -> cgroup:[4026531835]
lrwxrwxrwx 1 cizixs cizixs 0 12月 21 15:36 ipc -> ipc:[4026531839]
lrwxrwxrwx 1 cizixs cizixs 0 12月 21 15:36 mnt -> mnt:[4026531840]
lrwxrwxrwx 1 cizixs cizixs 0 12月 21 15:36 net -> net:[4026531969]
lrwxrwxrwx 1 cizixs cizixs 0 12月 21 15:36 pid -> pid:[4026531836]
lrwxrwxrwx 1 cizixs cizixs 0 12月 21 15:36 user -> user:[4026531837]
lrwxrwxrwx 1 cizixs cizixs 0 12月 21 15:36 uts -> uts:[4026531838]
```

-   容器的 PID namespace 情况如下：父进程是可以看到容器进程（子进程）的 pid（8，9，10）但容器进程只能看到自己的 pid（1，2，3）

![namespace pid](/images/docker/namespace-pid.png#center)

-   容器的 Mount namespace 情况如下：和 PID 不同，mount 的隔离有时是我们不想要的，比如系统中插入了新磁盘，虽然每个 namespace 都重新 mount， 这个可以通过 peer group 共享挂载信息

![namespace mount](/images/docker/namespace-mount.png#center)

-   容器的 Net namespace 情况如下：容器进程仍需和外界连续，这时需要额外建立 Virtual Network，不同的厂商有不同的技术路线，例如[Docker 的实现](https://github.com/moby/libnetwork/blob/master/docs/design.md)（容器间通信是个重点，但具体技术细节这里就不展开）

![namespace net](/images/docker/namespace-net.png#center)

### CGroup

容器进程共同占用了宿主机器的物理资源，所以有了隔离，还需要有资源限制，而 Linux 通过 CGroup 来定义资源大小并按 CGroup 来分配资源给进程。

✦ CGroup namespace 是 Linux4.6 以后才支持的新 namespace。没有 CGroup namespace 前，容器中一旦挂载 CGroup，便可以修改整全局的 CGroup 配置。有了 CGroup namespace 后，每个 namespace 中的进程都有自己的 CGroup 文件系统视图，增强了安全性，同时也让容器迁移更加方便。

CGroups 的特点是：

```md
-   CGroups 的 api 以一个伪文件系统的实现方式，用户的程序可以通过文件系统实现 CGroups 的组件管理
-   CGroups 的组件管理操作单元可以细粒度到线程级别，另外用户可以创建和销毁 CGroups，从而实现资源载分配和再利用
-   所有资源管理的功能都以子系统的方式实现，接口统一子任务创建之初与其父任务处于同一个 CGroups 的控制组
```

四大功能：

```md
-   资源限制：可以对任务使用的资源总额进行限制
-   优先级分配：通过分配的 cpu 时间片数量以及磁盘 IO 带宽大小，实际上相当于控制了任务运行优先级
-   资源统计：可以统计系统的资源使用量，如 cpu 时长，内存用量等
-   任务控制：CGroups 可以对任务执行挂起、恢复等操作
```

使用 CGroup 的方式有几种：

```md
-   使用 CGroup 提供的虚拟文件系统，直接通过创建、读写和删除目录、文件来控制 CGroup
-   使用命令行工具，比如 libcgroup 包提供的 cgcreate、cgexec、cgclassify 命令
-   使用 rules engine daemon 提供的配置文件
-   当然，systemd、lxc、docker 这些封装了 CGroup 的软件也能让你通过它们定义的接口控制 CGroup 的内容
```

### rootfs

Linux 万事皆为 file，或者叫 rootfs（根文件系统）. rootfs 不仅具有普通文件系统的存储数据文件的功能，还包含了一个操作系统所需要的文件，配置和目录，其它的文件系统才能依次加载到 root 下，但并不包含系统内核。 在 Linux 中，文件和内核是分开存放的，操作系统只有在开启启动时才会加载指定的内核。rootfs 包含一般我们熟知的 /bin，/sbin，/dev，/etc，/var，/proc 等目录：

![linux file system](/images/linux/cheatsheet-linux-fs.jpeg#center)

```md
-   根文件系统提供了根目录“/”；
-   init 进程的应用程序必须运行在根文件系统上；
-   linux 挂载分区时所依赖的信息存放于根文件系统/etc/fstab 这个文件中；
-   shell 命令程序必须运行在根文件系统上，譬如 ls、cd 等命令；
```

在容器内，也应该看到完全独立的 rootfs，而且不会受到宿主机以及其他容器的影响。这个针对容器 rootfs，就叫做容器镜像（对整个根目录文件系统的镜像），所有的容器都会共享宿主机上操作系统的内核。在镜像内，打包的不仅仅是应用，还有所需要的依赖，都被封装在一起。这就解决了无论是在哪，应用都可以很好的运行的原因。

不光这样，rootfs 还解决了可重用性的问题，想象这个场景，你通过 rootfs 打包了一个包含 java 环境的 centOS 镜像（java 应用），别人需要在容器内跑一个 apache 服务器，那么他是否需要为 apache 从头开始搭建 centOS 环境呢？Docker 镜像的设计中， 在解决这个问题时，引入了一个叫层的概念（通过 AUTOFS、OverlayFS 等文件系统技术来支持），如其名，OverlayFS 可以把不同的文件，一层一层的叠加在一起，如果有重复的文件（看作被修过的文件），后面的层覆盖前面的：每次针对 rootfs 的修改，都只保存增量的内容，这样不同的镜像之间相同的层只需一份（例如 java 应用和 apache 服务底层的 centOS），创造性的解决了镜像的制作，共享，存储，打包，传送等问题，否则整个 rootfs 相对 java 应用庞大不少。

镜像另一个采用到的技术就是 UnionFS（Union File System），2004 年由纽约州立大学石溪分校开发，它可以把多个目录(也叫分支)内容联合挂载到同一个目录下，而目录的物理位置是分开的。Docker 支持的 UnionFS 包括 OverlayFS，AUFS，devicemapper，vfs 以及 btrfs 等，Docker 在 Linux3.18 之后版本基本默认用 OverlayFS2。启动容器的时候 Docker 把镜像挂载到一个目录，作为容器的根文件系统。

-   不同的文件源（层）  
    ![overlay](/images/docker/overlay1.png#center)

-   依次进行 merge  
    ![overlay](/images/docker/overlay2.png#center)

-   最终的镜像（mount 在/tmp/overlay 下）  
    ![overlay](/images/docker/overlay3.png#center)

关于 AUFS，OvelayFS，具体的文件结构，参考底部的链接。

容器镜像技术是 Docker 公司的重大贡献，Docker 镜像的制作是通过 Docker File 完成：

```bash
FROM ubuntu:14.04
ADD run.sh /
VOLUME /data
CMD ["./run.sh"]
......
```

![docker image](/images/docker/docker-image.png#center)

## Docker

首先要明确一点，Docker 指的不是一个东西，它可能是：Docker CLI，Docker File，Docker Daemon，Docker Engine，Docker Hub，等等，从开源项目，变成产品名称，后来直接变成公司名称，由于容器因 Docker 而名声鹊起起，很多时候变成了容器的代名词 - 容器就像 Java，Docker 就像 JDK，其实是 Sun JDK，Java 实现还有 IBM JDK，Open JDK，等。Docker 的核心就是实现容器的构建与运行，但随之膨胀，加入了各种东西，加上各路人马的争夺，技术和各种术语非常混乱，Docker 的内部实现前后经历了很大变动，从 LXC 转到 runc。runc 就是一个命令行工具，直接调用内核/libcontainer 创建和运行一个容器进程，相当于一个轻量化的容器 runtime。runc 由 Docker 贡献给社区，目的是实现容器 runtime 的标准化。

Docker 的核心架构（2019）：

![docker architecture](/images/docker/docker-architecture.png#center)

-   <span style="color: orange">Docker Client</span>：接收 docker 命令并发送至 Docker daemon，可以通过以下三种方式和 Docker daemon 通信：tcp://host:port, unix://path_to_socket, fd://socketfd

-   <span style="color: orange">Docker Daemeon</span>：监听任何创建或运行容器以及其它容器相关的 Docker API 请求，其中的 Engine 是 Docker 架构中的运行引擎，同时也 Docker 运行的核心模块，它扮演 Docker container 存储仓库的角色，并且通过执行 job 的方式来操纵管理这些容器。Job 可以认为是 Docker 架构中 Engine 内部最基本的工作执行单元。，Docker 可以做的每一项工作，都可以抽象为一个 job，其设计与 Unix 进程相仿，比如说：Job 有一个名称，有参数，有环境变量，有标准的输入输出，有错误处理，有返回状态等。

-   <span style="color: orange">Docker Registries</span>：存放 Docker image 的地方，默认的是 [Docker Hub](https://hub.docker.com/)（公有仓库），私有仓库则需 Docker Registry 软件单独建立如 Harbor，镜像按照[repository]:[tag]来精确定义。

-   <span style="color: orange">graphitedriver</span>，<span style="color: orange">networkdriver</span>，<span style="color: orange">execdriver</span>：驱动模块，通过 driver，Docker 可以实现对不同 Docker 容器执行环境的定制。

-   <span style="color: orange">libcontainer</span>：一个 Go 语言设计实现的库，设计初衷是希望该库可以不依靠任何依赖，直接访问内核中与容器相关的 API，Docker 通过调用 libcontainer，而最终操纵容器的 namespace、cgroups、apparmor、网络设备以及防火墙规则等。

Docker 创建和运行容器的大致流程：

![docker 2019](/images/docker/architecture_2019.svg#center)

```cmd
1. dockerd 接收到post请求：Container Create
    1.1 dockerd 找到镜像文件 image
    1.2 产生相应的 container object
    1.3 文件系统创建给该容器使用的目录
2. dockerd 接收到post请求：Container Start
    2.1 为该容器创建 OCI spec
    2.2 调用 containerd 执行 create container
        2.2.1 containerd 把该 spec 存如内部数据库
    2.3 调用 containerd 执行 start container
        2.3.1 containerd 发出一个创建任务
            2.3.1.1 该任务通过shim脚本调用runc创建容器
        2.3.2 containerd 发出一个启动任务
            2.3.1.1 该任务通过shim脚本调用runc启动容器
        2.3.3 containerd/shim 会一直监控容器进程直至进程结束
```

上述流程在 containerd 框架下的直观视图：

![docker flow](/images/docker/docker-flow.png#center)

理清底层的概念原理后，具体的 Docker 使用，参考底部的链接。

## 容器革命

在我看来容器带来了两个革命性的东西：

> build faster, test faster, deploy faster, update faster, recover faster

-   To reusable code: **libraries**

    -   code once，import to every project

-   To reuse binary: **java**

    -   compile once，executable everywhere

-   To reuse local environment: **virtual machine**

    -   create once，share for every team member

-   To reuse infrastructure: **devops tools**

    -   define once，provision somewhere

-   To re-use immutable, deployable, runnable artifact: **container**

    -   build once, deploy everywhere & run (without installation/configuration)

容器带来革命性的软件打包方式：app 和所有的 dependencies 都打包在一起 - a single binary，具有**consistent，portable，immutable，versionable**的特性，朝着 everything as code 演化。

> Identical environment is crucial for delivering high quality software

开发高质量少 BUG 的软件的关键问题在于 - 如何确保开发与生产环境的一致性。传统做法是建立开发、调试、生产环境，在软件复杂化的情况下，整个团队如果共享单一开发环境非常不高效。同时要支持多版本，多环境，多配置的情况下，这变成了一个 O(n^2) 的问题，传统的做法根本 o 行不通。所以容器不只是一个新的软件打包方式，还是一项让我们开发高质量软件的重要技术。

---

🐳 容器就是受隔离和资源限制的进程，镜像就是其静态文件，VM 可以做到物理或 OS 级别的隔离，但容器共享 OS，是进程级别的隔离。Docker 提供了容器打包，运行，管理等一系列工具。

㊫ [Docker 背后的内核知识——cgroups 资源限制](https://www.infoq.cn/article/docker-kernel-knowledge-cgroups-resource-isolation/)  
㊫ [Cgroups, namespaces, and beyond: what are containers made from?](https://www.youtube.com/watch?v=sK5i-N34im8)  
㊫ [DOCKER 基础技术：AUFS](https://coolshell.cn/articles/17061.html)  
㊫ [深入理解 overlayfs（一）：初识](https://blog.csdn.net/luckyapple1028/article/details/77916194)  
㊫ [深入理解 overlayfs（二）：使用与原理分析](https://blog.csdn.net/luckyapple1028/article/details/78075358)  
㊫ 关于容器 runtime，参考 [Ian Lewis container-runtime-series](https://www.ianlewis.org/en/tag/container-runtime-series)  
㊫ 在线动手学习 Docker：[Play with Docker](https://www.docker.com/play-with-docker) 以及 [Katacoda](https://katacoda.com/courses/container-runtimes)

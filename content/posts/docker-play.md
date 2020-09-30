+++
title = "Docker 实操（Mac）"
date = 2020-08-30T05:08:47+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["docker"]
toc = true
+++

<!--more-->

前提准备：<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [Docker/容器介绍（2020.9 更新）](/posts/docker101/)

![docker cheatsheet](/images/docker/cheatsheet-docker.png)

## Docker Desktop 安装

Docker 桌面版的安装比较简单，下载后直接拖拽至 Applications：

![docker download](/images/docker/docker-download.png)  
![docker install](/images/docker/docker-install.png)

然后运行，在 Dashboard，可以看到还没有任何的容器：

![docker download](/images/docker/docker-dashboard.png)  
![docker install](/images/docker/no-container.png)

## Hello World in Docker

> 1. 运行 “docker run hello-world”
> 2. Docker client 通知 Docker daemon 运行 hello-world 镜像
> 3. Docker dameon 下载 hello-world 镜像（默认 latest 版本）

先登录 Docker Hub：  
![docker signin](/images/docker/docker-signin.png)

```bash
MySpace/k8slab/docker-play
[I] ➜ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:7f0a9f93b4aa3022c3a4c147a449bf11e0941a1fd0bf4a8e6c9408b2600777c5
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

再次打开 Docker Dashboard：  
![docker hello-world](/images/docker/hello-world.png)

> 4. 从 hello-world 镜像创建容器
> 5. 运行 hello-world 容器

```bash
# docker create：创建hello-world容器，镜像已经在本地，所以不需要下载，返回容器id
MySpace/k8slab/docker-play took 7s
[I] ➜ docker create hello-world
c1f055d2af524458ff3c05291f24a4702074e1173cc07e8eed15fd862d941cee

# docker run：运行hello-world容器
MySpace/k8slab/docker-play
[I] ➜ docker start c1f055d2af52
c1f055d2af52

# docker ps：显示容器进程，全部都已停止，注意容器的名称（NAMES栏）没有指明的话，将会被随机产生
MySpace/k8slab/docker-play
[I] ➜ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
c1f055d2af52        hello-world         "/hello"            24 seconds ago      Exited (0) 9 seconds ago                       hopeful_chatelet
a80072819373        hello-world         "/hello"            9 minutes ago       Exited (0) 9 minutes ago                       angry_hypatia

# docker rm：删除容器
MySpace/k8slab/docker-play
[I] ➜ docker rm a80072819373
a80072819373

# docker run -it：“交互状态” 下运行python容器，python镜像被下载
MySpace/k8slab/docker-play
[I] ➜ docker run -it python
Unable to find image 'python:latest' locally
latest: Pulling from library/python
d6ff36c9ec48: Pull complete
c958d65b3090: Pull complete
edaf0a6b092f: Pull complete
80931cf68816: Pull complete
7dc5581457b1: Pull complete
87013dc371d5: Pull complete
dbb5b2d86fe3: Pull complete
4cb6f1e38c2d: Pull complete
c2df8846f270: Pull complete
Digest: sha256:bc765f71aaa90648de6cfa356ec201d50549031a244f48f8f477f386517c5d1b
Status: Downloaded newer image for python:latest
Python 3.8.5 (default, Sep  1 2020, 18:44:24)
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("Hello World!");
Hello World!
>>> exit();

MySpace/k8slab/docker-play took 1m 3s
[I] ➜ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
4aa9202113e5        python              "python3"           32 seconds ago      Exited (0) 5 seconds ago                       flamboyant_moser
c1f055d2af52        hello-world         "/hello"            3 minutes ago       Exited (0) 3 minutes ago                       hopeful_chatelet

# docker run -d：以 “detach” 方式运行 redis，变成 daemon 进程
MySpace/k8slab/docker-play
[I] ➜ docker run -d redis
Unable to find image 'redis:latest' locally
latest: Pulling from library/redis
bf5952930446: Pull complete
911b8422b695: Pull complete
093b947e0ade: Pull complete
c7616728f575: Pull complete
61a55f107028: Pull complete
0ee3e0cb4e07: Pull complete
Digest: sha256:933c6c01829165885ea8468d87f71127b1cb76a711311e6c63708097e92ee3d1
Status: Downloaded newer image for redis:latest
25704fb05729b249afa8d68a155c710d28cfbfcf02bd97c09736ed5a1086fbf4

# docker exec：在容器进程里运行命令 redis-cli，ctrl+c 退出，注意 redis 容器进程依旧在
MySpace/k8slab/docker-play
[I] ➜ docker exec -it 25704fb05729 redis-cli
127.0.0.1:6379>

# docker log：查看容器进程的输出 - stdout
MySpace/k8slab/docker-play
[I] ➜ docker logs 25704fb05729
1:C 06 Sep 2020 12:57:11.264 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 06 Sep 2020 12:57:11.264 # Redis version=6.0.7, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 06 Sep 2020 12:57:11.264 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 06 Sep 2020 12:57:11.265 * Running mode=standalone, port=6379.
1:M 06 Sep 2020 12:57:11.265 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 06 Sep 2020 12:57:11.265 # Server initialized
1:M 06 Sep 2020 12:57:11.265 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1:M 06 Sep 2020 12:57:11.266 * Ready to accept connections

# redis容器 25704fb05729 的状态依旧是 “Up”
MySpace/k8slab/docker-play
[I] ➜ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
25704fb05729        redis               "docker-entrypoint.s…"   3 minutes ago       Up 3 minutes                6379/tcp            compassionate_mcnulty
4aa9202113e5        python              "python3"                14 minutes ago      Exited (0) 14 minutes ago                       flamboyant_moser
c1f055d2af52        hello-world         "/hello"                 17 minutes ago      Exited (0) 17 minutes ago                       hopeful_chatelet

# docker stop：停止容器进程
MySpace/k8slab/docker-play
[I] ➜ docker stop 25704fb05729
25704fb05729

MySpace/k8slab/docker-play
[I] ➜ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
25704fb05729        redis               "docker-entrypoint.s…"   3 minutes ago       Exited (0) 2 seconds ago                        compassionate_mcnulty
4aa9202113e5        python              "python3"                14 minutes ago      Exited (0) 14 minutes ago                       flamboyant_moser
c1f055d2af52        hello-world         "/hello"                 17 minutes ago      Exited (0) 17 minutes ago                       hopeful_chatelet

# docker image ls：列出本地的镜像
MySpace/k8slab/docker-play
[I] ➜ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
redis               latest              41de2cc0b30e        4 days ago          104MB
python              latest              a7cda474cef4        4 days ago          882MB
hello-world         latest              bf756fb1ae65        8 months ago        13.3kB

# docker system prune：清除容器等
MySpace/k8slab/docker-play
[I] ➜ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
Deleted Containers:
25704fb05729b249afa8d68a155c710d28cfbfcf02bd97c09736ed5a1086fbf4
4aa9202113e53a351f5d97510f65d8cc5ed09c721224af3ca7762203dcb795c7
c1f055d2af524458ff3c05291f24a4702074e1173cc07e8eed15fd862d941cee

MySpace/k8slab/docker-play
[I] ➜ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

docker 之前的命令没有分组，随着命令行越来越多，后来才开始分子命令，重要的几个：

-   <span style="color: orange">docker container</span>：管理容器生命周期，如 create，exec，kill，pause，restart，rm，run，start，stop，unpause
-   <span style="color: orange">docker image</span>：管理镜像，如 build，push，pull，inspect，rm
-   <span style="color: orange">docker network</span>：管理 docker 网络
-   <span style="color: orange">docker volume</span>：管理存储
-   <span style="color: orange">docker service，docker swarm，docker stack</span>：管理单个或多个 service

Kubernetes 早已一统容器管理的江湖，这里就略去 docker 这部分的高级功能。

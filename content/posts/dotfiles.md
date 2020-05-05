+++
title = "如何保存 dotfiles"
date = 2017-09-25T20:27:53+08:00
readingTime = true
categories = ["DevOps"]
tags = []
toc = false
+++

dotfiles - 你肯定知道我说的是什么，不然就不要往下看了。

<!--more-->

#### 本地初始化

```bash
$ git init --bare $HOME/.mdot
$ alias mdot='/usr/bin/git --git-dir=$HOME/.mdot/ --work-tree=$HOME'
$ mdot config --local status.showUntrackedFiles no
$ echo "alias mdot='/usr/bin/git --git-dir=$HOME/.mdot/ --work-tree=$HOME'" >> $HOME/.zshrc
$ echo ".mdot/" >> $HOME/.gitignore
$ mdot remote add origin git@github-fastzhong:fastzhong/mdot.git
```

-   建立一个 ["Git base repository"](http://blog.csdn.net/pcplayer/article/details/72784096) （ <font color="yellow">.mdot</font>），用这个 Git 仓库跟踪所有的 dotfiles
-   建立一个方便的 git 命令行 alias （<font color="yellow">mdot</font>）
-   "local" 和通常版本控制情况不同，默认下不显示没有跟踪的文件状态（跟踪文件必须先显式指明）
-   把 alias 放入 .zshrc（或者 .bashrc）
-   让 git 忽略 .mdot/，因为 .mdot/ 本身是仓库
-   加入到 Github

#### 接下来就可以开始跟踪 dotfiles

例如:

```bash
cd .ssh
mdot status
mdot add config
mdot commit -m "add ssh config"
mdot push -u origin master
```

#### 另一台电脑

安装时，也很简单，先把 mdot 仓库克隆到本地：

```bash
$ git clone --bare https://github.com/fastzhong/mdot.git $HOME/.mdot
```

然后重复初始化的步骤，最后 checkout：

```bash
$ alias mdot='/usr/bin/git --git-dir=$HOME/.mdot/ --work-tree=$HOME'
$ mdot config --local status.showUntrackedFiles no
$ echo "alias mdot='/usr/bin/git --git-dir=$HOME/.mdot/ --work-tree=$HOME'" >> $HOME/.zshrc
$ echo ".mdot/" >> .gitignore
$ mdot checkout
```

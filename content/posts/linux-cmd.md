+++
title = "Linux 命令行功夫"
date = 2010-01-01T13:08:47+08:00
readingTime = true
categories = ["工具"]
tags = ["功夫系列", "linux"]
toc = true
draft = true
+++

命令行功夫是个修炼的无底洞，也是老司机的“开车”技巧，你值得拥有

<!--more-->

## ⌨️ 常用快捷键

**_光标移动_**

```bash
-   Ctrl + a ：移到命令行首
-   Ctrl + b ：按字符后移（左向）
-   Ctrl + e ：移到命令行尾
-   Ctrl + f ：按字符前移（右向）
-   Alt + b  ：按单词后移（左向）
-   Alt + f  ：按单词前移（右向）
```

**_编辑命令_**

```bash
-   Ctrl + d ：删除光标处的字符
-   Ctrl + h ：删除光标前的字符
-   Ctrl + k ：从光标处删除至命令行尾
-   Ctrl + t ：交换光标处和之前的字符
-   Ctrl + u ：从光标处删除至命令行首
-   Ctrl + w ：从光标处删除至字首
-   Ctrl + xx：在命令行首和光标之间移动
-   Ctrl + y ：粘贴至光标后
-   Alt + c  ：从光标处更改为首字母大写的单词
-   Alt + d  ：从光标处删除至字尾
-   Alt + l  ：从光标处更改为全部小写的单词
-   Alt + t  ：交换光标处和之前的单词
-   Alt + u  ：从光标处更改为全部大写的单词
-   Alt + Backspace：与 Ctrl + w ~~相同~~类似，分隔符有些差别
-   ctrl+shift+c, ctrl+shift+v
```

**_历史命令_**

```bash
-   Ctrl + g：从历史搜索模式退出
-   Ctrl + n：历史中的下一条命令
-   Ctrl + p：历史中的上一条命令
-   Ctrl + r：逆向搜索命令历史
-   Alt + . ：使用上一条命令的最后一个参数
```

**_控制命令_**

```bash
-   Ctrl + c：终止命令
-   Ctrl + l：清屏
-   Ctrl + o：执行当前命令，并选择上一条命令
-   Ctrl + q：允许屏幕输出
-   Ctrl + s：阻止屏幕输出
-   Ctrl + z：挂起命令
```

## ⏩ 常用 alias

## ㊙ ️ 常用命令 & 技巧

### ❕ 命令

```bash
-   !!        ：执行上一题命令，不需要重敲， 可以配合 sudo
-   !blah     ：执行最近的以 blah 开头的命令，如 !ls
-   !blah:p   ：同上，仅打印输出，而不执行
-   !$        ：上一条命令的最后一个参数，与 Alt + . 相同
-   !$:p      ：同上，仅打印输出，而不执行
-   !*        ：上一条命令的所有参数
-   !*:p      ：同上，仅打印输出，而不执行
-   ^blah     ：删除上一条命令中的 blah
-   ^blah^foo ：将上一条命令中的 blah 替换为 foo
-   ^blah^foo^：将上一条命令中所有的 blah 都替换为 foo
```

### misc.

-   cd：回到 user 的 home 目录, 和 cd ～，cd \$HOME 等效

-   cd -：回到前一个目录

-   ssh 自动登陆

-   background 执行 fg bg

-   monitor

### 🗄️/proc

https://opensource.com/article/20/4/proc-filesystem

## misc.

ssh

vi/vim

## 资源

-   [the art of command line 命令行的艺术](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)：一文说尽命令行
-   [awesome macos command line](https://github.com/nusr/awesome-macos-command-line-zh)：收集了很多有趣的 Mac 终端命令
-   [awesome shell](https://unix-shell.zeef.com/caleb.xu)：
    A curated list of awesome command-line frameworks, toolkits, guides and gizmos

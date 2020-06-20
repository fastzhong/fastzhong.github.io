+++
title = "Git 功夫 - 图解Git"
date = 2020-02-01T13:08:47+08:00
readingTime = true
categories = ["DevOps"]
tags = ["功夫系列", "git"]
toc = true
draft = true
+++

git 的基本知识

<!--more-->

小姐姐太给力
[CS Visualized: Useful Git Commands](https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1)

## ⚙️ 配置

-   .gitattributes 处理换行符

换行符在 Linux 和 Windows 上是不同的：

Unix(newline): _\n_
Windows(carriage return + line feed): _\r\n_

和网上很多意见相反，`不建议使用 core.autocrlf`

\* text = auto

-   用 Git-LFS 处理二进制文件（大文件）

Git 不区分所谓的二进制文件的，关键是大的二进制文件，例如 image、video，如果经常更改的化，由于 Git 的工作机理会让 Git 的 repo 非常的庞大，而这些大文件的历史版本是不需要的。

## ⌨️ 快捷键

💡tips & tricks

git reflog

git recover

## 进阶

    -   [Udemy：Git & GitHub Bootcamp & Integration with most popular IDEs](https://www.udemy.com/git-bootcamp-with-github-learn-step-by-step/)
    -   [‍Udemy：Git a Web Developer Job: Mastering the Modern Workflow](https://www.udemy.com/git-a-web-developer-job-mastering-the-modern-workflow/)
    -   [‍Udemy：Git Complete: The definitive, step-by-step guide to Git](https://www.udemy.com/git-complete/)
    -   [‍Udemy：Learn Git by Doing: A step-by-step guide to version control](https://www.udemy.com/learn-git-by-doing-a-step-by-step-guide-to-version-control/)

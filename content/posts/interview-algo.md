+++
title = "算法编程面试指南"
date = 2001-01-01T13:08:47+08:00
readingTime = true
categories = ["算法"]
tags = ["面试指南系列", "leetcode"]
toc = true
draft = true
+++

冲着算法编程面试去的，不是学校计算机系里学的那种

<!--more-->

## 101 数据结构与算法课程

如果没学过或者学了忘记的，可以参考以下课程，大部分我看过，都是高手，强烈推荐：

-   慕课网 [liuyubobobo 的算法课 ](https://www.imooc.com/t/108955) ：有好几个课程，从数据结构，到算法面试，到算法应用，讲得非常接地气，可以补补基础，非常推荐

-   极客时间贾超 [70 天的算法训练营 ](https://u.geekbang.org/subject/algorithm/1000343) ：网上非常出名的课程，知识点覆盖全面，系统性讲解 ⏤ 如何练，怎们练，正在跟着一遍下来

-   极客时间王争的专栏 [数据结构与算法之美 ](https://time.geekbang.org/column/intro/126)：数据结构与算法学习经验

-   [九章算法](https://www.jiuzhang.com/)：很多不同的课 ⏤ 基础班、强化班、高频、动态规划，老师都是大 🐮，学生也有很多博士

-   [Data Structures Easy to Advanced Course - Full Tutorial from a Google Engineer](https://www.youtube.com/watch?v=RBSGKlAvoiM)

-   [Data Structures - Computer Science Course for Beginners](https://www.youtube.com/watch?v=zg9ih6SVACc)：Course created by Steven from NullPointer Exception，学习最常见的 12 种数据结构。

以上课程，b 站、油管、网盘全部都有免费，自行寻找。

MIT，Stanford，CMU，Coursera，Edx，Udacity 上都有大学的数据结构与算法的公开课，虽然很好，但学习时间较长，不走学术和研究路线的话不建议采用。

可以结合刷题来巩固知识点。如果没有编程基础，推荐使用 Python 来学习和刷题。

## 201 如何刷题

### 突击策略

-   前两百多道都是经典题，按着标签刷可以举一反三，例如， 先刷热题 HOT 200，再刷精选 TOP 面试题，之后刷其他的题。
-   easy/hard 一般不会考，碰到 hard 的就缴枪自认倒霉吧
-   一些算法是冷门，同样如果万一碰到了就自认倒霉吧

![leetcode algo](/images/algo/leetcode-algo.png)  
![leetcode top](/images/algo/leetcode-top.png)

> 此策略乃不得已为之，不建议采纳。

### 推荐策略 🔥

1. 基础：Leetcode -> Explore -> Learn 针对各个数据结构和算法的分类学习 + 练习:

![leetcode learn](/images/algo/leetcode-learn.png)

中文的资料更多：[https://leetcode-cn.com/leetbook/](https://leetcode-cn.com/leetbook/)

2. 针对每个 topic 强化刷题，包括套路和特定类型的问题：Leetcode -> Problems -> Tags 选择 topics，参考下面的“精华题/刷题列表”

3. 针对某个具体公司刷题：Leetcode -> Problems -> Tags 选择公司

### 刷题方法

极客算法训练营推荐的 <font color="orange">五遍刷题法/五毒神掌</font>：

```md
-   刷题第一遍
    -   5~10 分钟读题加思考
    -   然后直接看解法。注意！多解法，比较解法优劣
    -   背诵、默写好的解法
-   刷题第二遍
    -   马上自己写 -> 提交到 leetcode 反复 debug 至通过
    -   多种解法比较、体会 -> 优化，leetcode 有统计执行时间和内存消耗
-   刷题第三遍
    -   过了一天后，再重复做题
    -   不同解法的熟练程度 -> 专项练习
-   刷题第四遍
    -   过了一周再反复回来练习，同时对于不熟练的题目再进行专项练习
-   刷题第五遍
    -   面试前一周恢复性训练
```

## 202 精华题/刷题列表 💡

-   [图灵星球：LeetCode 精华题目列表【刷题规划系列】](https://turingplanet.org/2020/09/18/leetcode_planning_list/)

-   [LeetCode 刷题攻略：配思维导图，100+经典算法题目刷题顺序、经典算法模板、共 40w 字的详细图解，以及难点视频题解。按照刷题攻略上的顺序来刷题，让你在算法学习上不再迷茫](https://github.com/youngyangyang04/leetcode-master)

-   [算法模板（Go）：算法模板，最科学的刷题方式，最快速的刷题路径，一个月从入门到 offer，你值得拥有](https://greyireland.gitbook.io/algorithm-pattern/)

-   [小浩算法：目前共完成 140+ 道高频面试算法题目，全部采用漫画图解的方式](https://www.geekxh.com/)

-   [算法珠玑：一个最精简的题库，150 道左右](https://github.com/soulmachine/algorithm-essentials)

-   [CS-Notes](http://www.cyc2018.xyz/%E7%AE%97%E6%B3%95/Leetcode%20%E9%A2%98%E8%A7%A3/Leetcode%20%E9%A2%98%E8%A7%A3%20-%20%E7%9B%AE%E5%BD%95.html)

-   [人人都能看的懂：左神--左程云 2021 版【LeetCode 算法面试刷题教程】](https://www.bilibili.com/video/BV1854y1Y7qX)

## 203 套路 & 模版 💡

-   [turningplanet LeetCode 刷题系列 (Java)](https://turingplanet.org/leetcode%E5%88%B7%E9%A2%98%E7%B3%BB%E5%88%97/)

-   [AlgoWiki 力扣刷题模板](https://ojeveryday.github.io/AlgoWiki/#/)

-   [labuladong 的算法小抄](https://labuladong.gitbook.io/algo/)

-   [liweiwei1419](https://www.liwei.party/categories/)

-   [Algorithms and Coding Interviews](https://medium.com/algorithms-and-leetcode)

## 204 题解

### 各路大神作业

-   [力扣加加 - 努力做西湖区最好的算法题解](https://leetcode-solution-leetcode-pp.gitbook.io/leetcode-solution/)
-   Java/Go/C++: [LeetCode-book：LeetCode 1~400 题总结 + 专题知识点&题型总结](https://github.com/huxiaoman7/leetcodebook)
-   Java: [作者先后面试了近三十多家互联网公司（Java）](https://github.com/yuanguangxin/LeetCode)
-   Go: [Solutions to LeetCode by Go, 100% test coverage, runtime beats 100% / LeetCode 题解](https://books.halfrost.com/leetcode/)
-   C: [LeetCode in pure C](https://github.com/begeekmyfriend/leetcode)

### 中文讲解（油管）

-   💁🏻‍♂️ [花花酱 LeetCode 视频讲解](https://docs.google.com/spreadsheets/d/1HV-iKgYJL4DpSOX1M-HcYG1cmz5CQxoduLOdEoTnIXo/edit#gid=0) 及其网站：[https://zxi.mytechroad.com/blog/](https://zxi.mytechroad.com/blog/)
-   💁🏻‍♂️ [basketwangCoding](https://www.youtube.com/playlist?list=PLH8TFsY0qnE2R9kf_9vahNY6pG9601z_4)（Java 版）
-   💁🏻‍♂️ [LeetCode - AlgoCasts 讲解](https://www.youtube.com/playlist?list=PLIUC4_Q-00WdRvpPsVPWrCQcKX9NKdQvo)
-   💁🏻‍♂️ [古城算法](https://www.youtube.com/c/%E5%8F%A4%E5%9F%8E%E7%AE%97%E6%B3%95/featured)
-   💁🏻‍♂️ [FuFu LeetCode](https://www.youtube.com/channel/UCCMpGENpr93ENbfdinP3QeQ/playlists)

### 英文讲解（课程）

-   [Zero to Master Master the Coding Interview: Big Tech (FAANG) Interviews](https://academy.zerotomastery.io/p/master-the-coding-interview-faang-interview-prep)
-   [Back to Back SWE](https://backtobackswe.com/) （130 道）
-   [AlgoExpert](https://www.algoexpert.io/product)（100 道）
-   [InterviewPro](https://www.algoexpert.io/product)（100 道）
-   [Online Bootcamp for Coding Interviews](https://interviewcamp.io/)

## 刷题利器

### [VS Code leetcode 插件](https://github.com/jdneo/vscode-leetcode/blob/master/docs/README_zh-CN.md)

1. 唤出 leetcode  
   ![leetcode](/images/vscode/leetcode.png)
2. 先 Switch Endpoint 选“中国” or “US”，两边 leetcode 不同  
   ![leetcode endpoint](/images/vscode/leetcode-endpoint.png)
3. Sign in：⚠️ 注意 leetcode US 已经不能支持账号 Sign in，必须通过 cookie（从 Chrome 浏览器中 login，然后把完整的 cookie 拷贝过来）
   ![leetcode cookie](/images/vscode/leetcode-cookie.webp)
   ![leetcode cookie](/images/vscode/leetcode-cookie.png)
4. 接下来点击最右边导航条上 leetcode 标签
   ![leetcode demo](/images/vscode/leetcode-demo.gif)
5. 开刷
   ![leetcode submit](/images/vscode/leetcode-submit.png)

### 力扣刷题插件

[链接](https://mp.weixin.qq.com/s/3hvFH2Ry1hoJoUe5plEElg)

---

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 小结：

> -   如果有编程经验的，用什么语言刷题一点都不重要，因为语言层面只用到最最基本的东西（int，string，array，if/else，for/while 等）
> -   刷题之前先把数据结构和基本算法过一遍，把基础打牢
> -   刷题没有捷径，只有策略和积累，无论按照怎样的顺序、策略来刷题，想要灭考官，都需要经历 `题海战术`
> -   只刷题还不够，一定要回顾总结，掌握套路 & 模板

如果你不想做 CRUD 的程序员，但每天写的东西就是在屎山上堆屎，刷题就是另一个路径，是一种刻意训练，提高编程功底，开拓思维 🤫 听说，很多高手也在“悄悄”的刷。

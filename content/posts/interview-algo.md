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

如果没学过或者学了忘记的，可以参考以下课程（前五个我看过），都是高手，强烈推荐：

-   [Data Structures Easy to Advanced Course - Full Tutorial from a Google Engineer（英文）](https://www.youtube.com/watch?v=RBSGKlAvoiM)：数据结构基础课程，前 Google 员工和 ACM 顶级高手，讲得很好。

-   [Data Structures - Computer Science Course for Beginners（英文）](https://www.youtube.com/watch?v=zg9ih6SVACc)：Course created by Steven from NullPointer Exception，学习最常见的 12 种数据结构。

-   慕课网 [liuyubobobo 的算法课 ](https://www.imooc.com/t/108955) ：有好几个课程，从数据结构，到算法面试，到算法应用，讲得非常接地气，可以补补基础，非常推荐

-   极客时间贾超 [70 天的算法训练营 ](https://u.geekbang.org/subject/algorithm/1000343) ：网上非常出名的课程，知识点覆盖全面，系统性讲解 ⏤ 如何练，怎们练，正在跟着一遍下来

-   极客时间王争的专栏 [数据结构与算法之美 ](https://time.geekbang.org/column/intro/126)：数据结构与算法学习经验

-   [人人都能看的懂：左神--左程云 2021 版【LeetCode 算法面试刷题教程】](https://www.bilibili.com/video/BV1854y1Y7qX)

-   [九章算法](https://www.jiuzhang.com/)：很多不同的课 ⏤ 基础班、强化班、高频、动态规划，老师都是大 🐮，学生也有很多博士

以上课程，b 站、油管、网盘全部都有免费 🆓，自行寻找。

MIT，Stanford，CMU，Coursera，Edx，Udacity 上都有大学的数据结构与算法的公开课，虽然很好，但学习时间较长，不走学术和研究路线的话不建议采用。

可以结合刷题来巩固知识点。如果没有编程基础，推荐使用 Python 来学习和刷题。

## 201 如何刷题

### 策略 A：突击

-   前两百多道都是经典题，先刷热题 HOT 100 ～ 200，再刷精选 TOP 面试题，之后刷其他的题。
-   easy/hard 一般不会考，碰到 hard 的就缴枪自认倒霉吧
-   一些算法是冷门，同样如果万一碰到了就自认倒霉吧

![leetcode algo](/images/algo/leetcode-algo.png)  
![leetcode top](/images/algo/leetcode-top.png)

### 策略 B：循序渐进

👍🏻 👍🏻 👍🏻

1. 基础：Leetcode -> Explore -> Learn 针对各个数据结构和算法的分类学习 + 练习:

![leetcode learn](/images/algo/leetcode-learn.png)

leetcode 中文站的资料就更多：[https://leetcode-cn.com/leetbook/](https://leetcode-cn.com/leetbook/)

![leetcode-cn](/images/algo/leetcode-cn.png)

算法基础好的或者学校刚毕业的可以跳过这部分，直接刷题。

2. 针对每个 topic 强化刷题，包括套路和特定类型的问题：Leetcode -> Problems -> Tags 选择 topics，参考下面的“精华题/刷题列表”

3. 面试前针对某个具体公司刷题：Leetcode -> Problems -> Tags 选择公司

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

## 202 精华题/刷题列表

👍🏻 👍🏻 👍🏻

-   [图灵星球：LeetCode 精华题目列表【刷题规划系列】](https://turingplanet.org/2020/09/18/leetcode_planning_list/)

> 星友们好呀~ 我是以诺，一位 Cal Poly Pomona 的在读研究生。我爱编程，爱科技，也是个爱沉浸在科幻世界里的白日梦患者哈哈 :P  
> [刷题方法论](https://turingplanet.org/2020/09/20/leetcode_how_to/)

-   [LeetCode 刷题攻略：配思维导图，100+经典算法题目刷题顺序、经典算法模板、共 40w 字的详细图解，以及难点视频题解。按照刷题攻略上的顺序来刷题，让你在算法学习上不再迷茫](https://github.com/youngyangyang04/leetcode-master)

> 大家好，我是程序员 Carl，哈工大师兄，ACM 校赛、黑龙江省赛、东北四省赛金牌、亚洲区域赛铜牌获得者，先后在腾讯和百度从事后端技术研发，CSDN 博客专家。 对算法和 C++后端技术有一定的见解，利用工作之余重新刷 leetcode。

-   [算法模板（Go）：算法模板，最科学的刷题方式，最快速的刷题路径，一个月从入门到 offer，你值得拥有](https://greyireland.gitbook.io/algorithm-pattern/)

> 此项目是自己找工作时，从 0 开始刷 LeetCode 的心得记录，通过各种刷题文章、专栏、视频等总结了一套自己的刷题模板。这个模板主要是介绍了一些通用的刷题模板，以及一些常见问题，如到底要刷多少题，按什么顺序来刷题，如何提高刷题效率等。

-   [小浩算法：目前共完成 140+ 道高频面试算法题目，全部采用漫画图解的方式](https://www.geekxh.com/)

> 小浩算法是我在疫情期间完成的一部图解算法题典！目前共完成 140+ 道高频面试算法题目，总计 40w 字，全部采用漫画图解的方式，简单易懂。拥有近 10w 读者！

-   [算法珠玑：一个最精简的题库，150 道左右](https://github.com/soulmachine/algorithm-essentials)

> 举一反三，以少胜多，告别题海战术 @灵魂机器

-   [算法·进阶石（algorithm-stone）—— 进击的每一步！（C++）](https://github.com/acm-clan/algorithm-stone)

> 本项目是公众号《ACM 算法日常》整理的一个算法学习路线图。

-   [CS-Notes](http://www.cyc2018.xyz/%E7%AE%97%E6%B3%95/Leetcode%20%E9%A2%98%E8%A7%A3/Leetcode%20%E9%A2%98%E8%A7%A3%20-%20%E7%9B%AE%E5%BD%95.html)

> 本文从 Leetcode 中精选大概 200 左右的题目，去除了某些繁杂但是没有多少算法思想的题目，同时保留了面试中经常被问到的经典题目。

## 203 套路 & 模版

👍🏻 👍🏻 👍🏻

-   [turningplanet LeetCode 刷题系列 (Java)](https://turingplanet.org/leetcode%E5%88%B7%E9%A2%98%E7%B3%BB%E5%88%97/)

-   [AlgoWiki 力扣刷题模板](https://ojeveryday.github.io/AlgoWiki/#/)

-   [九章算法 - 令狐冲的算法小抄](https://github.com/ninechapter-algorithm/linghu-algorithm-templete)

-   [labuladong 的算法小抄](https://labuladong.gitbook.io/algo/)

-   [liweiwei1419](https://www.liwei.party/categories/)

-   [Algorithms and Coding Interviews](https://medium.com/algorithms-and-leetcode)

## 204 题解

### 各路大神作业

-   Java/Go/C++: [LeetCode-book：LeetCode 1~400 题总结 + 专题知识点&题型总结](https://github.com/huxiaoman7/leetcodebook)
-   Java: [作者先后面试了近三十多家互联网公司](https://github.com/yuanguangxin/LeetCode)
-   Go: [Solutions to LeetCode by Go, 100% test coverage, runtime beats 100% / LeetCode 题解](https://books.halfrost.com/leetcode/)
-   C: [LeetCode in pure C](https://github.com/begeekmyfriend/leetcode)
-   Javascrip/Python：[力扣加加 - 努力做西湖区最好的算法题解](https://leetcode-solution-leetcode-pp.gitbook.io/leetcode-solution/)
-   Java：[Cspiration 1-300](https://github.com/JojoYang666/Leetcode-1-300) [301 - 600](https://github.com/JojoYang666/Leetcode-301-600)
-   各种算法的实现（几乎涵盖所有的编程语言）：[TheAlgorithms](https://the-algorithms.com/)

### 中文讲解

-   💁🏻‍♂️ 花花酱

    -   油管：[花花酱 LeetCode 视频讲解](https://docs.google.com/spreadsheets/d/1HV-iKgYJL4DpSOX1M-HcYG1cmz5CQxoduLOdEoTnIXo/edit#gid=0)
    -   B 站：[花花酱 LeetCode](https://space.bilibili.com/9880352)
    -   网站：[https://zxi.mytechroad.com/blog/](https://zxi.mytechroad.com/blog/)

    > 花花酱讲解 LeetCode 题目已经很多年了，一直更新，属于老字号。他针对不同题型开辟了各种算法专题，涵盖了 LeetCode 上很广泛的各类题型，比较适合刚刚入门的同学进行系统性分块学习。而且花花酱更新频率比较高，平时的 contest 也会参加，会分享一些解题思路。

-   💁🏻‍♂️ 篮子王

    -   油管：[basketwangCoding](https://www.youtube.com/playlist?list=PLH8TFsY0qnE2R9kf_9vahNY6pG9601z_4)（Java 版）
    -   B 站：[basketwangCoding](https://space.bilibili.com/108657656)

    > 篮子王可能是油管上最好的中文视频讲解:P。篮子王不更新了，不过上传的解题视频中，已经覆盖了很多高频题。

-   💁🏻‍♂️ [油管：FuFu LeetCode](https://www.youtube.com/channel/UCCMpGENpr93ENbfdinP3QeQ/playlists)

-   💁🏻‍♂️ [油管：古城算法](https://www.youtube.com/c/%E5%8F%A4%E5%9F%8E%E7%AE%97%E6%B3%95/featured)

-   💁🏻‍♂️ 爱学习的饲养员
    -   B 站：[手把手带你刷 Leetcode 力扣｜各个击破数据结构和算法](https://www.bilibili.com/video/BV1sy4y1q79M)
    -   B 站：[Leetcode 力扣必备算法知识和练习题｜手画图解版](https://www.bilibili.com/video/BV1xt4y1e7q4)
    -   B 站：[Leetcode 力扣 1-300 题视频讲解合集｜手画图解版+代码](https://www.bilibili.com/video/BV1xa411A76q)
    -   B 站：[Leetcode 力扣 301+题视频讲解合集｜手画图解版+代码](https://www.bilibili.com/video/BV1eK4y1j7fT)

### 英文讲解（课程）

👍🏻 👍🏻 👍🏻 非常好但需要花钱购买 🤑 🤑 🤑

-   [Zero to Master Master the Coding Interview: Big Tech (FAANG) Interviews](https://academy.zerotomastery.io/p/master-the-coding-interview-faang-interview-prep)
-   [Back to Back SWE](https://backtobackswe.com/) （130 道）
-   [AlgoExpert](https://www.algoexpert.io/product)（100 道）
-   [InterviewPro](https://www.algoexpert.io/product)（100 道）
-   [Online Bootcamp for Coding Interviews](https://interviewcamp.io/)
-   [Interview Cake](https://www.interviewcake.com/)

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

### 力扣刷题插件（Chrome）

[链接](https://mp.weixin.qq.com/s/3hvFH2Ry1hoJoUe5plEElg)

---

leetcode 是最好的，觉得还吃不饱的话 😅 还有 [InterviewBit](https://www.interviewbit.com/)，[GG](https://www.geeksforgeeks.org/)，[Google's Coding Competitions](https://codingcompetitions.withgoogle.com/)，[TopCoder](https://www.topcoder.com/challenges/?pageIndex=1)，[HackerRank](https://www.hackerrank.com/domains)，[《挑战程序设计竞赛》](<https://github.com/guanhui07/studyFiles/blob/master/%E7%AE%97%E6%B3%95/%E6%8C%91%E6%88%98%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1%E7%AB%9E%E8%B5%9B(%E7%AC%AC2%E7%89%88).pdf>)，[《算法竞赛进阶指南》](https://github.com/lydrainbowcat/tedukuri)等等。

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 小结：

> -   如果有编程经验的，用什么语言刷题一点都不重要，因为语言层面只用到最最基本的东西（int，string，array，if/else，for/while 等）
> -   刷题之前先把数据结构和基本算法过一遍，把基础打牢
> -   刷题没有捷径，只有策略和积累，无论按照怎样的顺序、策略来刷题，想要灭考官，都需要经历 `题海战术`
> -   只刷题还不够，一定要回顾总结，掌握套路 & 模板

如果你不想做 CRUD 的程序员，但每天写的东西就是在屎山上堆屎，刷题就是另一个路径，是一种刻意训练，提高编程功底，开拓思维，听说，很多高手也在“悄悄”的刷。

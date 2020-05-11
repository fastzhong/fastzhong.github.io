+++
title = "算法编程面试 I：指南"
date = 2010-07-01T13:08:47+08:00
readingTime = true
categories = ["算法"]
tags = ["leetcode"]
toc = true
draft = true
+++

不服就干，对，就是冲着算法编程面试去的，不是学校计算机系里学的那种 ☕️

<!--more-->

## 课程

都是我看过的，都是高高手 👍🏻，**_强烈推荐_** ：

-   慕课网 🎬 [liuyubobobo 的算法课 ](https://www.imooc.com/t/108955) ：有好几个课程，从数据结构，到算法面试，到算法应用，讲得非常接地气，可以补补基础，我非常喜欢

-   极客时间贾超 🎬 [70 天的算法训练营 ](https://u.geekbang.org/subject/algorithm/1000343) ：网上非常出名的课程，知识点覆盖非常全面，系统性讲解和练习，如何练，怎们练，正在跟着一遍下来

-   极客时间王争的专栏 📝 [数据结构与算法之美 ](https://time.geekbang.org/column/intro/126)：算法学习经验

-   [九章算法](https://www.jiuzhang.com/)：很多不同的课 ⏤ 基础班、强化班、高频、动态规划，老师都是大 🐮，学生也有很多博士

-   拉勾苏勇 🎬[300 分钟搞定数据结构与算法 ](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=3#/content) ：刷题面试重点提示，最后冲刺复习之用

以上课程，b 站、油管、网盘全部都有 🆓，不要问自己找。

## leetcode 刷题

💡 刻意练习，刷题是必须的。leetcode 是主力，刷好，面试不是问题。

> 无论按照怎样的顺序来刷题，都需要经历 `题海战术`

### 刷题指南 & 姿势

-   极客算法训练营：五遍刷题法（<font color="yellow">五毒神掌</font>）

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

-   先刷热门题，前两百多道都是经典题，可以举一反三。 在 [https://leetcode-cn.com/problemset/all/](https://leetcode-cn.com/problemset/all/) 页面的右侧。先刷热题 HOT 100，再刷精选 TOP 面试题，之后刷其他的题。按 tag 分类刷，

    -   初期：熟悉掌握各种基本工具  
        按难度分类，做简单题，广度优先，直接看答案，学最优解，补基础

    -   中期：熟悉各种类别题目解题思路  
        按类别分类，广度优先，背经典题，模版，建立思维体系

    -   后期：做新题，总结老题，一题多解，多题同解
        按类别分类，深度优先，

    -   面试前：做面经，别纠结
        按公司分类，因公司而异

#### leetcode & VS Code

[插件在此](https://github.com/jdneo/vscode-leetcode/blob/master/docs/README_zh-CN.md)

### 题目分类 & 题解 


解题套路：较长，另开一篇 <i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [算法编程面试 III：刷题总结&模版](/posts/interview-algo-iii/)

https://docs.google.com/spreadsheets/d/1yRCOJ8KysRVkq0O9IlDriT01tC6lzPapmFO4PCmDJQA/edit#gid=126913158

#### 讲解





-   [cspiration Leetcode 800 题目讲解](https://www.youtube.com/channel/UCTWuRL33U8xBPqk3LehXjFw/playlists) 牛人刷了 3000 道，已转行专业教人刷题
-   [JS 老毕：人人都能看得懂的 Leetcode 刷题教程合集](https://www.bilibili.com/video/BV1wA411b7qZ?p=1)
-   [笔记哥讲题集](https://www.youtube.com/playlist?list=PLbi6Uu6v-4YsjcyE004ifZbsir72kGKsy)



## 📚 刷题参考书

LeetCode 刷完了，可以看看：

-   [算法竞赛进阶指南]()

*   [剑指 Offer（第 2 版）⏬](https://drive.google.com/open?id=118MNIjDQGf-jAL8GOP5YSO1PPeZebBlQ)  
     <font color="yellow">豆瓣评分 8.3，0.7K+人评价</font> 这本面试宝典上面涵盖了很多经典的算法面试题。 题解：
    ![剑指 Offer](/images/algo/book-jian.zhi.offer.jpg)

-   [程序员代码面试指南：IT 名企算法与数据结构题目最优解（第 2 版）⏬](https://drive.google.com/open?id=1Vnsqij9A0SnJ2ZNlMQrf5sVvw1_OsSxJ)  
    <font color="yellow">豆瓣评分 8.7，0.2K+人评价</font> 据说题目相比于《剑指 offer》 来说要难很多，题目涵盖面相比于《剑指 offer》也更加全面。全书一共有将近 300 道真实出现过的经典代码面试题。作者，从 2010 年起专注刷题至今，拥有 10 年算法刷题经验。题解：  
    ![程序员代码面试指南](/images/algo/book-zuo.cheng.yun.jpeg)

---

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 最后，刷题对提高还是有好处的，但毕竟是针对面试的，还是要回到长期正规的学习，学以致用应中。

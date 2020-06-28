+++
title = "算法编程面试 I：Leetcode 指南"
date = 2020-01-01T13:08:47+08:00
readingTime = true
categories = ["算法"]
tags = ["面试指南系列", "leetcode"]
toc = true
draft = true
+++

冲着算法编程面试去的，不是学校计算机系里学的那种

<!--more-->

## 基础

针对算法面试的数据结构与算法课 🎬 ，都是我看过或正在看的，都是高高手 👍🏻，强烈推荐：

-   慕课网 [liuyubobobo 的算法课 ](https://www.imooc.com/t/108955) ：有好几个课程，从数据结构，到算法面试，到算法应用，讲得非常接地气，可以补补基础，我非常喜欢

-   极客时间贾超 [70 天的算法训练营 ](https://u.geekbang.org/subject/algorithm/1000343) ：网上非常出名的课程，知识点覆盖全面，系统性讲解 ⏤ 如何练，怎们练，正在跟着一遍下来

-   极客时间王争的专栏 [数据结构与算法之美 ](https://time.geekbang.org/column/intro/126)：数据结构与算法学习经验

-   拉勾公瑾 [重学数据结构与算法](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=185#/content)：数据结构与算法学习经验

-   拉勾苏勇 [300 分钟搞定数据结构与算法 ](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=3#/content) ：刷题面试重点提示，数据结构与算法重点复习

*   [九章算法](https://www.jiuzhang.com/)：很多不同的课 ⏤ 基础班、强化班、高频、动态规划，老师都是大 🐮，学生也有很多博士

以上课程，b 站、油管、网盘全部都有 🆓，不要问自己找。

💡 自己总结了一篇 <i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [算法编程面试 II：基本数据结构 & 算法](/posts/interview-algo-ii/)

## 刷题（leetcode）

> 刻意练习，刷题是必须的，无论按照怎样的顺序、策略来刷题，想要灭考官，都需要经历 🔥`题海战术`🔥

### 刷题方法

极客算法训练营五遍刷题法： <font color="yellow">五毒神掌</font>

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

### 刷题策略

![leetcode tag](/images/algo/leetcode-tag.webp)

💡 题目太多了，还是要有策略的：先刷热门题，前两百多道都是经典题，可以举一反三，例如， 先刷热题 HOT 200，再刷精选 TOP 面试题，之后刷其他的题。（在 [https://leetcode-cn.com/problemset/all/](https://leetcode-cn.com/problemset/all/) 页面的右侧可选不同的热门题。）

```md
1. 前期：先熟悉掌握基本算法结构，算法面试课先补基础，配合做做简单题

2. 初中期：熟悉各种类别解题思路，按类别分类/tag 刷题，同一类型强化，开始直接看答案，学最优解，背经典题，模版，建立思维体系

3. 后期：做新题，总结老题，一题多解，多题同解，补缺补漏，深度优先

4. 面试前：做面经，别纠结，按公司分类，因公司而异
```

短期应试策略 ⏤ 若短期只为应试冲刺的话：

-   easy/hard 都不会考，略过，碰到 hard 的就缴枪自认倒霉吧 😓
-   一些算法是冷门，同样如果万一碰到了就自认倒霉吧 😓  
     ![leetcode algo](/images/algo/leetcode-algo.png)
    ![leetcode top](/images/algo/leetcode-top.png)

### 刷题利器

[VS Code 的 leetcode 插件](https://github.com/jdneo/vscode-leetcode/blob/master/docs/README_zh-CN.md)

1. 唤出 leetcode  
   ![leetcode](/images/vscode/leetcode.png)
2. 先 Switch Endpoint 选“中” or “美”，两边 leetcode 不同  
   ![leetcode endpoint](/images/vscode/leetcode-endpoint.png)
3. 接下来点击最右边导航条上 leetcode 标签
   ![leetcode demo](/images/vscode/leetcode-demo.gif)
4. 刷

### 题目分类和题解

#### 题解

#### 题目讲解

💁🏻‍♂️ [用动画的形式呈现解 LeetCode 题目的思路](https://github.com/MisterBooo/LeetCodeAnimation) 不佩服不行

💁🏻‍♂️ [cspiration](https://www.youtube.com/channel/UCTWuRL33U8xBPqk3LehXjFw/playlists) 超级刷题牛人，刷了 3000 道，已创业专业教人刷题

💁🏻‍♂️ [花花酱](https://www.youtube.com/user/xxfflower/playlists) 也是个刷题牛人

💁🏻‍♂️ [绵羊教授 CodeSheep](https://www.youtube.com/playlist?list=PLgkTb_uYkq5f6mI52NZv68QTb6Ui7omWX) 中英文讲解

💁🏻‍♂️ [basketwangCoding](https://www.youtube.com/playlist?list=PLH8TFsY0qnE2R9kf_9vahNY6pG9601z_4) Java 版

💁🏻‍♂️ [人人都能看得懂的 Leetcode 刷题教程合集](https://www.bilibili.com/video/BV1wA411b7qZ) Javascript 版

### 高频面试题讲解

参考题库：

### 解题套路、模版、总结

💡 自己总结了一篇 <i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [算法编程面试 III：解题套路、模版、总结](/posts/interview-algo-iii/)

---

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 最后，如果你不想做 CRUD 的程序员，但每天写的东西就是在屎山上堆屎，刷题就是另一个路径，是一种刻意训练，对提高编程功底，开拓思维，非常有好处的 ⏤ 其实，很多高手好像也在“悄悄”的刷。刷题毕竟是针对面试的，刷题是打基础，最好是学以致用，挑战真正的困难。

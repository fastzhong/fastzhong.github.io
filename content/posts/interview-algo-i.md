+++
title = "算法编程面试 I：Leetcode 刷题指南"
date = 2020-08-31T13:08:47+08:00
readingTime = true
categories = ["算法"]
tags = ["面试指南系列", "leetcode"]
toc = true
draft = true
+++

冲着算法编程面试去的，不是学校计算机系里学的那种

<!--more-->

## 课程

针对算法面试的数据结构与算法课，大部分我看过，都是高高手，强烈推荐：

-   👍🏻 慕课网 [liuyubobobo 的算法课 ](https://www.imooc.com/t/108955) ：有好几个课程，从数据结构，到算法面试，到算法应用，讲得非常接地气，可以补补基础，我非常推荐

-   👍🏻 极客时间贾超 [70 天的算法训练营 ](https://u.geekbang.org/subject/algorithm/1000343) ：网上非常出名的课程，知识点覆盖全面，系统性讲解 ⏤ 如何练，怎们练，正在跟着一遍下来

-   极客时间王争的专栏 [数据结构与算法之美 ](https://time.geekbang.org/column/intro/126)：数据结构与算法学习经验

-   拉勾公瑾 [重学数据结构与算法](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=185#/content)：数据结构与算法学习经验

-   拉勾苏勇 [300 分钟搞定数据结构与算法 ](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=3#/content) ：刷题面试重点提示，数据结构与算法重点复习

-   [九章算法](https://www.jiuzhang.com/)：很多不同的课 ⏤ 基础班、强化班、高频、动态规划，老师都是大 🐮，学生也有很多博士

以上课程，b 站、油管、网盘全部都有，不要问自己找。

总结了一篇 <i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [算法编程面试 II：数据结构 & 算法](/posts/interview-algo-ii/)

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


## 刷题策略

-   题目太多了，还是要有策略的：先刷热门题，前两百多道都是经典题，按着标签刷可以举一反三，例如， 先刷热题 HOT 200，再刷精选 TOP 面试题，之后刷其他的题。（在 [https://leetcode-cn.com/problemset/all/](https://leetcode-cn.com/problemset/all/) 页面的右侧可选不同的热门题。）
-   easy/hard 一般不会考，碰到 hard 的就缴枪自认倒霉吧
-   一些算法是冷门，同样如果万一碰到了就自认倒霉吧

![leetcode tag](/images/algo/leetcode-tag.webp)
![leetcode algo](/images/algo/leetcode-algo.png)  
![leetcode top](/images/algo/leetcode-top.png)

## 刷题方法

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

## 视频讲解

有不少，以下是比较出名的：

💁🏻‍♂️ [花花酱](https://www.youtube.com/user/xxfflower/playlists) 刷题牛人  
💁🏻‍♂️ [绵羊教授 CodeSheep](https://www.youtube.com/playlist?list=PLgkTb_uYkq5f6mI52NZv68QTb6Ui7omWX) 中英文讲解  
💁🏻‍♂️ [basketwangCoding](https://www.youtube.com/playlist?list=PLH8TFsY0qnE2R9kf_9vahNY6pG9601z_4) Java 版  
💁🏻‍♂️ [人人都能看得懂的 Leetcode 刷题教程合集](https://www.bilibili.com/video/BV1wA411b7qZ) Javascript 版  
💁🏻‍♂️ [cspiration](https://www.youtube.com/channel/UCTWuRL33U8xBPqk3LehXjFw/playlists) 刷题牛人，刷了 3000 道

## 刷题路径

🔥 重点来了，已经有现成的，适合小白上手，可以照着刷（都是按着同一类刷）：

-   [turningplanet LeetCode 刷题系列 (Java)](https://turingplanet.org/leetcode%E5%88%B7%E9%A2%98%E7%B3%BB%E5%88%97/)
-   [100 天摆脱算法小白](https://github.com/geekxh/Algorithm-100-Days)
-   [小浩算法：目前共完成 140+ 道高频面试算法题目，全部采用漫画图解的方式](https://www.geekxh.com/)
-   [算法模板（Go）：算法模板，最科学的刷题方式，最快速的刷题路径，一个月从入门到 offer，你值得拥有](https://greyireland.gitbook.io/algorithm-pattern/)
-   [算法珠玑：一个最精简的题库，150 道左右](https://github.com/soulmachine/algorithm-essentials)
-   [作者先后面试了近三十多家互联网公司（Java）](https://github.com/yuanguangxin/LeetCode)

下面是英文的：

-   [KAeducation：JavaScript & LeetCode The Interview Bootcamp](https://kaeducation.com/)
-   [KAeducation：Python & LeetCode The Interview Bootcamp](https://kaeducation.com/)
-   [Online Bootcamp for Coding Interviews](https://interviewcamp.io/)

以上是基础，然后可以逐步扩大题量，例如：

-   [LeetCode-book：LeetCode 1~400 题总结 + 专题知识点&题型总结（Java、Python、C++）](https://github.com/huxiaoman7/leetcodebook)
-   [Solutions to LeetCode by Go, 100% test coverage, runtime beats 100% / LeetCode 题解](https://books.halfrost.com/leetcode/)

同时熟练套路 & 模板，见下。

## 解题套路 & 模板

-   [labuladong 的算法小抄](https://github.com/labuladong/fucking-algorithm)
-   [五分钟学算法：用动画的形式呈现解 LeetCode 题目的思路](https://github.com/MisterBooo/LeetCodeAnimation)
-   [力扣加加](https://leetcode-solution.cn/)
-   [帅地](https://github.com/iamshuaidi/algo-basic)
-   [AlgoWiki 力扣刷题模板](https://ojeveryday.github.io/AlgoWiki/#/)

自己总结了一篇 <i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [算法编程面试 III：解题套路 & 模板](/posts/interview-algo-iii/)

---

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 小结：如果有编程经验的，用什么语言刷题一点都不重要，因为语言层面只用到最最基本的东西（int，string，array，if/else，for/while 等），难点还是在解题思路上

> -   刷题之前先把数据结构和基本算法过一遍，把基础打牢
> -   刷题没有捷径，只有策略和积累，无论按照怎样的顺序、策略来刷题，想要灭考官，都需要经历 `题海战术`
> -   只刷题还不够，一定要回顾总结，掌握套路 & 模板

如果你不想做 CRUD 的程序员，但每天写的东西就是在屎山上堆屎，刷题就是另一个路径，是一种刻意训练，提高编程功底，开拓思维 ⏤ 听说，很多高手也在“悄悄”的刷。

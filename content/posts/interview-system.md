+++
title = "系统设计面试的准备"
date = 2020-05-23T13:08:47+08:00
readingTime = true
categories = ["职业发展"]
tags = ["面试指南系列"]
toc = true
+++

系统设计的关键点在于知识面广 + 考虑全面，这需要经验做支撑，但通过多学习案例、套路是可以弥补的

<!--more-->

## 学习资料

<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [Learn how to design large-scale systems. Prep for the system design interview. Includes Anki flashcards.](https://github.com/donnemartin/system-design-primer/blob/master/README-zh-Hans.md)

<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [System design interview for IT companies](https://github.com/checkcheckzz/system-design-interview)

除了拿来面试，也是学习架构的大宝藏。

## 经典案例

突击 or 有架构经验的话，我觉得只要认真研读经典设计问题即可：

| 问题                                                                                |                                                                                                           <i class="fas fa-external-link-alt"></i>                                                                                                           |
| :---------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| <font color="honeydew">设计 Pastebin.com (或者 Bit.ly)</font>                       | [解答 1](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/pastebin/README-zh-Hans.md) <br/> [解答 2](https://www.educative.io/courses/grokking-the-system-design-interview/m2ygV4E81AR?affiliate_id=5073518643380224) |
| <font color="honeydew">设计 Twitter 时间线和搜索 (或者 Facebook feed 和搜索)</font> |                                                                      [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/twitter/README.md)                                                                       |
| <font color="honeydew">设计一个网页爬虫</font>                                      |                                                                    [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/web_crawler/README.md)                                                                     |
| <font color="honeydew">设计一个图片分享网站 Instantgram</font>                      |                                                                                  [解答](https://www.educative.io/courses/grokking-the-system-design-interview/m2yDVZnQ8lG)                                                                                   |
| <font color="honeydew">设计 Mint.com</font>                                         |                                                                        [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/mint/README.md)                                                                        |
| <font color="honeydew">为一个交易平台设计限价订单（limited order book）</font>      |                                                                    [解答](https://javarevisited.blogspot.com/2017/03/2-practical-data-structure-algorithm-interview-questions-java.html)                                                                     |
| <font color="honeydew">为一个社交网络设计数据结构</font>                            |                                                                    [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/social_graph/README.md)                                                                    |
| <font color="honeydew">为搜索引擎设计一个 key-value 储存</font>                     |                                                                    [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/query_cache/README.md)                                                                     |
| <font color="honeydew">通过分类特性设计 Amazon 的销售排名</font>                    |                                                                     [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/sales_rank/README.md)                                                                     |
| <font color="honeydew">在 AWS 上设计一个百万用户级别的系统</font>                   |                                                                    [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/scaling_aws/README.md)                                                                     |

## 答题策略

九章总结的 4S 分析法：

```md
-   Scenario 场景：需要设计哪些功能，到什么程度？
    -   who will use
    -   how many will use
    -   usage pattern
    -   use case not covered
    -   estimated throughput
    -   estimated latency
-   Service 服务：将大系统拆分为小服务
    -   api for read/write scenarios
    -   schema
-   Storage 存储：数据如何存储与访问
    -   data size
    -   read/write ratio
    -   read/write traffic
    -   rdbms/nosql
-   Scale 升级：解决缺陷，处理可能遇到的问题
    -   scaling the algorithm
    -   scaling the individual component
    -   memory cache
    -   DNS, CDN, reverse proxy, load balancer, ...
    -   async: message queue, back pressure, time & order, ...
    -   communiction: tcp, udp, rest, rpc, ...
```

## 技术点参考

<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [云原生时代|分布式系统设计知识图谱（内含 22 个知识点）](https://yq.aliyun.com/articles/719353)
![分布式系统设计知识图谱](/images/arch/techniques.png)

展开的话范围相当的广，只能平时积累，逐个突破。

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 番外：

<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [我理解的架构 & 架构师（兼谈微服务）](/posts/arch101)

<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp;[淘宝服务端高并发分布式架构演进之路](/posts/taobao/) ⏤ 其实是互联网应用的典型架构，架构是互通的，了解后至少懂得大方向。

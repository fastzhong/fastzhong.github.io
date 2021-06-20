+++
title = "系统设计面试指南"
date = 2020-05-23T13:08:47+08:00
readingTime = true
categories = ["职业发展"]
tags = ["面试指南系列"]
toc = true
+++

系统设计的关键点在于知识面广 + 考虑全面，这需要经验做支撑，但通过多学习案例、套路是可以弥补的

<!--more-->

## 学习资料

-   GitHub 上的资料 - 除了拿来面试，也是学习架构的大宝藏：

    -   <i class="fab fa-github" aria-hidden="true"></i>&nbsp; [The System Design Primer（中文翻译：系统设计入门）](https://github.com/donnemartin/system-design-primer/blob/master/README-zh-Hans.md)

    -   <i class="fab fa-github" aria-hidden="true"></i>&nbsp; [System design interview for IT companies](https://github.com/checkcheckzz/system-design-interview)

-   课程：

    -   [Educative: Scalability & System Design for Developers](https://www.educative.io/path/scalability-system-design) 💵

    -   [AlgoExpert: SystemsExpert](https://www.algoexpert.io/systems/product) 💵

    -   [Interview Daemon: System Design (Real World Examples)](https://interviewdaemon.com/courses/design-real-world-examples/) 🆓

## 经典案例

突击 or 有架构经验的话，我觉得只要认真研读经典设计问题即可：

| 问题                                                                           |                                  <i class="fab fa-github" aria-hidden="true"></i>&nbsp; GitHub                                   |                                                                                                                              <i class="fab fa-youtube"></i>&nbsp; Youtube                                                                                                                              |                                                                                                    <i class="fas fa-external-link-alt"></i>&nbsp;  web                                                                                                     |
| :----------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| <font color="honeydew">Twitter 时间线和搜索，Facebook feed</font>              |        [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/twitter/README.md)         |                                                        [花花酱](https://www.youtube.com/watch?v=PMCdWr6ejpw&list=PLLuMmzMTgVK4RuSJjXUxjeUt3-vSyA1Or&index=2&t=86s)<br/>[Tech Dummies Narendra L](https://www.youtube.com/watch?v=wYk0xPP_P_8)                                                         | [educative: Designing Twitter](https://www.educative.io/courses/grokking-the-system-design-interview/m2G48X18NDO)<br/>[educative: Designing Facebook’s Newsfeed](https://www.educative.io/courses/grokking-the-system-design-interview/gxpWJ3ZKYwl) |
| <font color="honeydew">Chat：WhatsApp</font>                                   |                                                                                                                                  |                                                                                                                       [Gaurav Sen](https://www.youtube.com/watch?v=vvhC64hQZMk)                                                                                                                       |                                                                                                                                                                                                                                                     |
| <font color="honeydew">图片分享：Instagram</font>                              |                    [解答](https://www.educative.io/courses/grokking-the-system-design-interview/m2yDVZnQ8lG)                     |                                                                                                                       [Gaurav Sen](https://www.youtube.com/watch?v=QmX2NPkJTKg)                                                                                                                       |                                                                           [educative](https://www.educative.io/courses/grokking-the-system-design-interview/m2yDVZnQ8lG)                                                                            |
| <font color="honeydew">电商：Amazon，eBay，Walmart</font>                      |      [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/scaling_aws/README.md)       |                                                                                                                       [codeKarle](https://www.youtube.com/watch?v=EpASu_1dUdE)                                                                                                                        |                                                                                                                                                                                                                                                     |
| <font color="honeydew">视频：YouTube，Netflix</font>                           |                                                                                                                                  | [花花酱](https://www.youtube.com/watch?v=mp-OSK6jm1c&list=PLLuMmzMTgVK4RuSJjXUxjeUt3-vSyA1Or&index=3&t=27s)<br/> [Gaurav Sen](https://www.youtube.com/watch?v=vvhC64hQZMk)<br/>[Tech Dummies Narendra L](https://www.youtube.com/watch?v=psQzyFfsUGU&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=5) |                                                                           [educative](https://www.educative.io/courses/grokking-the-system-design-interview/xV26VjZ7yMl)                                                                            |
| <font color="honeydew">视频会议：Zoom</font>                                   |                                                                                                                                  |                                                                                                                       [codeKarle](https://www.youtube.com/watch?v=G32ThJakeHk)                                                                                                                        |                                                                                                                                                                                                                                                     |
| <font color="honeydew">Uber 后台</font>                                        |                                                                                                                                  |                                                                                                                [Tech Dummies Narendra L](https://www.youtube.com/watch?v=umWABit-wbk)                                                                                                                 |            [Uber Architecture and System Design](https://medium.com/nerd-for-tech/uber-architecture-and-system-design-e8ac26690dfc)<br/> [educative](https://www.educative.io/courses/grokking-the-system-design-interview/YQVkjp548NM)             |
| <font color="honeydew">云存储服务：Dropbox，Google Doc，AWS S3</font>          |                                                                                                                                  |                                                              [Dropbox](https://www.youtube.com/watch?v=U0xTu6E2CT8)<br/>[Google Doc](https://www.youtube.com/watch?v=2auwirNBvGg)<br/>[S3](https://www.youtube.com/watch?v=UmWtcgC96X8)                                                               |                                                                           [educative](https://www.educative.io/courses/grokking-the-system-design-interview/m22Gymjp4mG)                                                                            |
| <font color="honeydew">Tiny URL：Pastebin.com，Bit.ly</font>                   | [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/pastebin/README-zh-Hans.md) <br/> |                                                                                                                [Tech Dummies Narendra L](https://www.youtube.com/watch?v=JQDHz72OA3c)                                                                                                                 |                                                            [educative](https://www.educative.io/courses/grokking-the-system-design-interview/m2ygV4E81AR?affiliate_id=5073518643380224)                                                             |
| <font color="honeydew">网页爬虫</font>                                         |      [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/web_crawler/README.md)       |                                                                                                                [Tech Dummies Narendra L](https://www.youtube.com/watch?v=BKZxZwUgL3Y)                                                                                                                 |                                                                           [educative](https://www.educative.io/courses/grokking-the-system-design-interview/NE5LpPrWrKv)                                                                            |
| <font color="honeydew">API Rate Limiter</font>                                 |      [解答](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/web_crawler/README.md)       |                                                                                                                       [Gaurav Sen](https://www.youtube.com/watch?v=xrizarXJgC8)                                                                                                                       |                                                                           [educative](https://www.educative.io/courses/grokking-the-system-design-interview/3jYKmrVAPGQ)                                                                            |
| <font color="honeydew">为一个交易平台设计限价订单（limited order book）</font> |      [解答](https://javarevisited.blogspot.com/2017/03/2-practical-data-structure-algorithm-interview-questions-java.html)       |                                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                     |

## 答题技巧

-   和面试官多沟通，给出任何细节前多问，沟通能力也是系统设计的考察点
-   由于系统设计可以多种多样，先确立一个简单解，以此为基础进行扩展
-   进行扩展时，一方面要了解需求，一方面要提出如此设计的原因和所做的 trade off

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

[<i class="fas fa-external-link-alt"></i>&nbsp; 云原生时代|分布式系统设计知识图谱（内含 22 个知识点）](https://yq.aliyun.com/articles/719353)
![分布式系统设计知识图谱](/images/arch/techniques.png)

展开的话范围相当的广，只能平时积累，逐个突破。

---

<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [我理解的架构 & 架构师（兼谈微服务）](/posts/sys101)

<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [淘宝服务端高并发分布式架构演进之路](/posts/sys-taobao/) ⏤ 互联网应用的典型架构，架构是互通的，了解后至少懂得大方向。

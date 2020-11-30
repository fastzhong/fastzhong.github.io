+++
title = "我理解的架构 & 架构师（兼谈微服务）"
date = 2018-10-06T13:18:02+08:00
readingTime = true
categories = ["系统设计 & 架构"]
tags = ["microservices"]
toc = true
+++

比较杂 🤔

<!--more-->

## 我理解的架构和架构师

从事商业性的系统开发超过二十年了，经手的 mission critical 系统最少也有十几，二十个，对自己最擅长的方面做个总结。

-   架构 architecture 来源于建筑行业，架构就是 blueprint 实施蓝图。软件架构的实质还是软件，也就是代码 ⏤ 架构师自己如何编码，只是没有全部亲自写出来。对于复杂系统，架构应该是系统设计之上更高级的抽象，我认为的架构，系统设计，代码应该是同源的，划分代码，组织代码，运行代码，支撑代码，等等，都是以代码为核心。

> `所谓架构和系统设计，其本质都是代码设计，也就是架构师给出的代码结构`

-   复杂系统的设计是需要有技术手段做支撑的，如果不懂得造好飞机如何设计好的飞机，所以架构师本身必须是个资深的程序员，熟悉各种技术手段，各个子系统，前后端，以及软件开发的全周期，必须是个全才，具备相当的技术“深度”与“宽度”。所以架构师必须爱好学习，持续不断的学习，持续编码，保持对最优秀技术的敏感度，才能设计出真正优秀的架构。和其它事物一样，学习、思考、实践 ⏤ 学习好的系统设计，思考后面的思想，实际中应用和解决问题，是提高水平的唯一方法。

> `复杂系统的设计是需要有具体软件技术做支撑，架构师必须持续不断的学习和应用各种前沿技术`

-   系统设计和软件如何编码一样，范围很广，基础理论杂，主要还是来源于实践经验，里面的很多设计决定和哲学相关。所以不仅仅是懂怎么做的，更多是体会为什么要这么设计所以原则很重要。架构和系统的本质还是编程，系统级别的编程，所以编程的一些经典原则也非常适用：

    -   <font color="orange">DRY</font>：Don’t repeat yourself  
        系统设计中你碰到的问题别人都已经碰到过了，向高手学习他们的解题方法。另外，别重复别人的错误，对于设计和架构，不止学技术，懂得来龙去脉非常重要。

    -   <font color="orange">KISS</font>：Keep it simple, stupid!  
        我更喜欢解读成 Keep it stupid, so it can be simple（笨方法往往是最好的方法）。

    -   <font color="orange">Avoid Creating a YAGNI</font>：You aren’t going to need it  
        类似 MVP（Minimum Viable Product）- 极简原则，可造可不造 ⏤ 别造， 可用可不用 ⏤ 别用，不知道怎么做 ⏤ 先研究，这在设计 API 时非常有用，同样可以应用在系统设计上。

    -   <font color="orange">Abstraction Principle</font>

    -   <font color="orange">Minimize Coupling</font>  
        解耦原则：复杂问题的解决方法来源解耦，解的过程就是深入认识问题的过程 ⏤ 如果代码的力量来源抽象，那么系统的威力就来源于解耦，这是两根大棒，但不要“滥用”（💡 抽象和解耦都是有代价的），得不断提高这方面的能力

    -   <font color="orange">Separation of Concerns</font>

    -   <font color="orange">Single Responsibility Principle</font>

    -   <font color="orange">Maximize Cohesion</font>  
        像搭积木一样，解决问题和写代码时尽量采用组合或延续的办法而不是从头开始，复用的威力

    -   <font color="orange">Open/Closed Principle</font>  
        完整性和灵活性是架构师和优秀程序猿时常考虑的问题，也是 NB 或菜鸟的区别所在

    -   <font color="orange">Don’t make me think</font>  
        把问题讲清楚，问题已经解决一大半了

    -   <font color="orange">Principle of least astonishment</font>  
        一切和想的一样，no news is good news

    -   <font color="orange">Avoid Premature Optimization</font>

> `系统设计更多的是艺术而不是科学，优良的系统设计遵循编程的原则和模式`

-   面对的是复杂系统的设计，通常没有固定的对错答案和所谓的 silver bullet。一个优秀架构师的成长需要经验积累，这和通常单项技术被动学习有所不同。一个优秀架构师最重要的能力是分析和抓住复杂事物的本质。例如五代战机的特点就是 4S - 隐身性能、超音速巡航、高机动型和超视距打击，五代战机的设计核心就是回答如何实现 4S。架构师最重要的能力还来源于深度思考，学习的不只是技术本身，还思考技术的来龙去脉，前因后果，思考解决问题的方法论 ⏤ 也就是套路，不断归纳和总结。

> `系统设计具有非常强烈的问题驱动，自我思考和实践的特点，必须能够回答系统的核心问题是什么，解决的关键是什么`

-   通常程序员只是一个人在编码，向组长负责。而架构师需要面对不同的 staleholder，带领一大群人编码完成项目。 一个优秀的架构师对系统设计考虑的问题必须非常全面，内部的，外部的，看得到的，看不到的，现在的，将来的，例如新技术的成熟度，实施该模块团队的技术储备，用户体验，项目的资金和时限，承受风险的能力，等等。需求是软件开发的原动力，商业、客户、技术都是在变化的，而一个系统的开发大都需要 2，3 年，所以架构师必须对需求有一定深度的理解和前瞻性，能够引领和超越客户、领导的提出需求，这些都影响架构的选型，扩展，冗余以及性能设计。越复杂的系统和商业需求越是考验一个架构师。高大上的系统还会区分出专门的商业架构，系统架构，基础设施架构，数据架构，安全架构等等，我本身的理解是系统架构师的认识要达到和商业架构师同一个层面，而优秀的架构师往往是两者合一。

> `系统设计是一个架构师对项目综合理解的体现`

## 系统架构图

软件系统的架构通常需要包含如下四类核心要素：
元素（elements）：将系统拆分为一组元素 - 模块、组件、结构体、子系统；
关系（relationships）：不同元素之间的关系 - 交互、依赖 、继承、组合、聚合；
属性（properties）：每个元素具备的属性 - 名称、职责、接口、实现限制等；
原理（principles）：为什么这么设计 - 拆分依据、设计原则、决策原因等。

SOLID 原则是一套比较经典且流行的架构原则（主要还是名字起得好）：

单一职责：与 Unix 哲学所倡导的“Do one thing and do it well”不谋而合；
开闭原则：用新增（扩展）来取代修改（破坏现有封装），这与函数式的 immutable 思想也有异曲同工之妙；
里式替换：父类能够出现的地方子类一定能够出现，这样它们之间才算是具备继承的“Is-A”关系；
接口隔离：不要让一个类依赖另一个类中用不到的接口，简单说就是最小化组件之间的接口依赖和耦合；
依赖反转：依赖抽象类与接口，而不是具体实现；让低层次模块依赖高层次模块的稳定抽象，实现解耦。
此外，我们做架构设计时也会尽量遵循如下一些原则（与上述 SOLID 原则在本质上也是相通的）：

正交性：架构同一层次拆分出的各组件之间，应该尽量保持正交，即彼此职责独立，边界清晰，没有重叠；
高内聚：同一组件内部应该是高度内聚的（cohesive），像是一个不可分割的整体（否则就应该拆开）；
低耦合：不同组件之间应该尽量减少耦合（coupling），既降低相互的变化影响，也能增强组件可复用性；
隔离变化：许多架构原则与模式的本质都是在隔离变化 —— 将预期可能变化的部分都隔离到一块，减少发生变化时受影响（需要修改代码、重新测试或产生故障隐患）的其他稳定部分。

所以，“4+1”到底是指什么？让我们来 Wiki 一下：“4+1”是一种视图模型（view model），可以通过多种共存的视图描述软件密集型系统的架构。这些视图基于不同项目干系人（利益相关者）的视点（viewpoint），例如：终端用户、开发者、系统工程师和项目经理。“4+1”由 4 种基础视图和一些经过挑选的用例或场景（即额外的“+1”视图）组成，各自的具体含义如下：

逻辑视图（Logical view）：描述系统为终端用户提供的功能，一般会通过UML中的类图和状态图来表示；

过程视图（Process view）：描述系统的动态行为，包括流程和交互等，一般会通过 UML 中的时序图、活动图和通讯图来表示；

开发视图（Development view）：从程序员的视角来阐述系统，也被称为“实现视图”，一般会通过 UML 中的组件图和包图来表示；

物理视图（Physical view）：从系统工程师的角度来描述系统，包括系统组件的物理拓扑、各组件之间的物理连接，也被称为“部署视图”，一般会通过 UML 中的部署图来表示；

场景（Scenarios）：通过一小组用例或场景来描述架构，包括系统中各种对象和进程之间的交互时序，也被称为“用例视图”。这些场景会被用于识别架构元素（architectural elements）以及阐述和验证整个架构设计，也可以被作为架构原型的测试起



## 主要的架构模式

✦ `客户端-服务器模式`  
最早的大型机系统所采用，从瘦客户端到富客户端，解决多个客户分享同一昂贵资源的问题，复杂处理全部集中在一处，简单直接，现在很多系统依旧采用的设计。  
![client-server](/images/arch/client-server.jpeg#center)

✦ `分层模式`  
著名代表就是三层架构，随着 Internet 的发展，后端变得复杂，主从模式分离出表示层（也称 UI 层）、应用层（也称服务层）、数据层；后来还发展出 middleware，中台等共享基础架构层。  
![3-tier](/images/arch/3-tier.jpg#center)

✦ `模型-视图-控制器模式（MVC）`  
解决应用层复杂的数据处理变化及数据展示关系，MVC 有多个变型，从后端 MVC 到前端 MVC。  
![mvc](/images/arch/mvc.jpg#center)

✦ `_主从模式_`  
这是由于高可用要求，这种模式由两部分组成，multicster 和 slaves，slave 通常作为 master 的备胎。  
![master-slave](/images/arch/master-slave.png#center)

✦ `Broker 模式`  
这是数量级的关系，单台机器已无法胜任，要求在多台机器上同时执行，需要一个中间代理人，由代理模组件管理如何发布处理到具体的处理节点上。  
![broker](/images/arch/broker.png#center)

✦ `事件总线模式`  
系统越来越复杂和庞大，需要解耦，有了总线，各组件可以拆分出来。有了事件总线，也就意为着采用事件监听和事件响应模式。  
![EBS](/images/arch/EBS.jpg#center)

✦ `Workflow/Pipeline 流水线模式`  
这个常见于系统集成和数据处理，Spring Integration、Mule 都是采用该模式。  
![pipeline](/images/arch/pipeline.png#center)

✦ `单体 🆚 微服务模式`  
近来流行的微服务模式
![microservice](/images/arch/microservice.png#center)

✦ `Sidecar (边车)模式`  
Kubernetes 为基础的微服务底层架构，把网络通信，安全，等从应用中剥离出来。
![sidecar](/images/arch/sidecar.png#center)

以上的架构都不新鲜，好坏关键还在于具体应用的技术以及对系统的理解和实践上。更多的参考微软文档：[Application design patterns](https://docs.microsoft.com/en-us/azure/architecture/patterns/)

## 分布式系统的学习

✦ 入门可参考：[面向分布式系统工程师的分布式系统理论（译）](/posts/distributed101/)

✦ 通读一遍 [Designing Data-Intensive Applications 中文版：数据密集型应用系统设计](https://drive.google.com/file/d/129k7s6JxdaX3ku-PoFm7i8HLdRAJl-t5/view?usp=sharing)，可对现代数据密集型系统采用的技术可以有个很好的了解：
![ 数据密集型应用系统设计](/images/arch/data-intensive-system.png#center)
此书有八百多个 Reference，所以也可以进一步细读。

题外：系统偏向以命令方式还是数据方式集成是个难点，很多架构师都没有想清楚，导致系统混乱不堪。

✦ [云原生时代|分布式系统设计知识图谱（内含 22 个知识点）](https://yq.aliyun.com/articles/719353)涵盖了大多数的技术点，可以以此为导向深挖。

✦ 名校开的分布式系统设计课，数据库系统课，等等，以及所列出的阅读清单（reading list），通常包括了各种经典论文，有事没事可以拿来读读，还有人主动翻译了：

💡[分布式系统(Distributed System)资料](https://github.com/ty4z2008/Qix/blob/master/ds.md)  
💡[分布式系统领域经典论文翻译集](http://duanple.com/?p=170)

[MIT 6.824: Distributed Systems](https://pdos.csail.mit.edu/6.824/schedule.html) 是经典课程，油管上有课程视频。

## 微服务是什么和为什么

4、5 年前就开始了所谓的微服务（Microservices），做过布道，做过大规模实施，下面以此为例，总结自己的思考和理解。

微服务是什么，众说纷纭，不过要说到名词解释，必须看 [Martin 的 Microservices 介绍](https://www.youtube.com/watch?v=2yko4TbC8cI):

> the microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API.

可惜听起来还是不甚了了。

我总结微服务有 4 个鲜明特点：

-   <font color="orange">设计解耦（design independently）/商业拆分</font>：这里讲的架构不是技术架构，而是商业逻辑架构，这在大层面上决定了软件如何拆分，这可以应对商业需求在情况下不明确或者比较复杂下，先找出"基本解"。design independently 接下来的好处就是各部分的架构是 **持续可演化性**（continuously evolutionary），而单体架构（monolithic），在商业不明确或比较复杂情况下，特别是创业公司，设计上无法完全确定下来，导致开发迟迟无法开动或完成；同时一个部分的变化会直接引发整体做出变化，这导致两种恶性后果，一是某一部分成为瓶颈拖累整体，另一个是开发后，大家都无法持续改进。例如典型的三层架构，很多表，很多代码都是和自身负责的模块无关；一个小改动整个项目需要重新构建；某个底层库由于各个模块都依赖，到了一定时候，各个模块的视角不同，有的模块想改进这个库或采用完全新的技术，但动不了了，成为技术瓶颈或死结，所谓的 legacy，重构无望，这时候只有等更高层做决定，推倒重来。注意微服务不只是技术拆分，因为，代码技术上是可以做到随意拆分，而企业或商业运作是不能随意拆分的，拆分需要符合体制和流程，微服务的系统最终由人来运作，所以不是所有的项目都适用微服务架构。

-   <font color="orange">技术解耦（implement independently）/多元化技术</font>：要体现微服务的最大威力一定是采用 polyglot，已有的软件解决方案可以直接采纳，管他是 javascript 还是 python 开发的，管他是自己开发还是他人已经提供的，也不管是通过 lib 或者还是 api。传统的 java 或 .net 包头包尾，一种数据库，一个通用数据层，要承担不同的技术考量，结果自然局部非最优解。技术解耦带来了技术革新，例如各种 nosql，而且在不同公司，不同项目中可实现重用，实现快速开发，宏观层面上发挥了最大效益：**“Service Endpoint first instead of APIs”**。所以微服务属于八仙过海各显神通，不再是单一的三层架构，从一层（如 serverless），到 n 层，都是可能的。

-   <font color="orange">资源解耦（sourcing independently）/游击队战术（coway rule & full stack developers）</font>：标准解释 ✓ 一个 service 团队的规模不超过两个 pizza。小团队才能带来快速灵活技术和快速迭代，“大象能跳舞”是个伪命题。要注意的是游击队战术不是适用所有的企业和商业模式，也不是所有的企业具备游击队的人员结构，所以微服务不是每个企业都适合的。

-   <font color="orange">部署&运行解耦（deploy&run independently）/自动化运维（devops）</font>：自动化运维（DevOps）不是微服务特有的，在现有的项目或系统上完全可以实施自动化运维，也可以看到巨大效益，CI/CD 在微服务提出之前已经存在了。但自动化运维是实施微服务的必备技术，拆分和 ployglot 后对自动化运维提出了很高的要求，一堆不同质的东西可以单独部署，升级，同时又要组合在一起无间隙运行。容器技术的出现，对微服务所要求的大规模自动化运维提供了必要的技术基础。

微服务的技术核心仍就是沿用“模块化”式思路设计整个系统，达到软件的 <font color="orange">可重用</font> 和 <font color="orange">可扩展</font> 。纵观开发历史，微服务不是什么新东西，仍旧是代码的拆分和集成，和 component-based 或 SOA 是一致的，只是层面不同，微服务不是针对小型软件或者单个项目层面，因为大部分的软件项目还没有等到解耦，拆分，长期演化成为”硬“需求，项目已经结束或者死掉啦。但反之，不同技术实力的团队，不同的项目，不同的地点，不同的时间，重复开发和实现，微服务解耦特性就能把软件的最大价值发挥出来，否则都是在做 silo application/project。所以微服务在政府，大公司或大型项目中推广和应用才能发挥其威力。一个 legacy 变成几十个 legacy，总体质量反而指数级下降。

另外，微服务还包含了开发团队的架构，运维的架构，这是和以往的纯系统架构概念非常不同的地方，也是实施微服务可能忽略的地方。<font color="orange">微服务同时锁定了以 service 为导向的开发和运维模式（service oriented development and operation）</font>。微服务一上来大家马上关注拆分，个人认为理念要先搞清楚，自动化运维和团队组织必须先，这是必要条件，同时对现有的单体架构帮助也甚大。对于单体架构的改造我认为首先考虑的应该不是如何完全拆分，而是合并，重用。

> `微服务的本质就是：分布式系统`

## 微服务的技术栈

如果在 IT 界呆得够久，了解分布式一路走来的历程，就会明白“微服务”从技术讲没有什么特殊的新问题，只是技术手段的不断翻新，技术点包括：

![ 微服务技术栈](/images/microservices/microservices-stack.jpg#center)

在我看来，注册，配置，数据一致性，链路跟踪，是微服务最基础和特有的关键点。

对应每个点都可以找到相应的具体技术，微服务的老大和先行者是 AWS，可惜它不开源。业界广泛的开源方案就是 Spring 系列的 Spring Boot & Spring Cloud，另一套是正在兴起是基于容器和 Kubernetes 的 服务网格（Service Mesh）。Spring Cloud 虽然是目前最成熟的方案，但从技术高度讲，我认为是开倒车或者说属于第一代的技术尝试（对比以前的 Corba，DCOM，J2EE，Web Services），不是发展大方向。

Cloud Native，以及以 Cloud Native 为基础的网格服务才是微服务的明天，参考：<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; [服务网格（Service Mesh ）FAQ](/posts/service-mesh-faq/)

🗓 分布式系统调用除 RPC 外就是消息系统支撑的 Event Driven Architecture ⏤ 异步是系统集成最主要也是最重要的手段，名词也是满天飞，ESB（Enterprise Service Bus），Event Sourcing，CQRS，Streaming Processing，还有更隐晦的 Reactive Systems 等等，有时间才好好整理。

## 微服务/分布式的坑

The three basic patterns are (1) decompensation – when the system exhausts its capacity to adapt as disturbances / challenges cascade; (2) working at cross-purposes – when roles exhibit behaviour that is locally adaptive but globally mal-adaptive; and (3) getting stuck in outdated behaviours – when the system over-relies on past successes

代码和工程的质量仍是灵魂，拆分和集成并不意味着可重用，可扩展就是必然。

> `如果你不能设计一个优良的单体系统，那么微服务也帮不上忙`

-   大规模的微服务实施往往变成一抓就死，一放就散。游击队战术的引入，各组人马自行乱搞，各种 style，违背了 KISS 和 DRY 的金典，引入更多更复杂的技术，反而使得整体性变得异常复杂，脆弱，低效和难以维护；
-   一个好的 API 设计已经如此不易，想要设计优良的、对外的、可复用的 service 对大多数小团队其实是无法胜任；
-   技术滥用或不慎用，滥用分布式调用 ⏤ 远程同步（RPC/REST） 或 远程异步（消息系统）从而导致：

    -   可读性下降、出错性上升：异步调用促使程序的碎片化，增加了流程处理和错误处理的复杂度，降低了程序可读性，响应速度，以及数据处理效率；
    -   性能下降：基于 Json 的 REST 还是序列/反序列 RPC 调用增加了数据的正确性和完整性判断，同时调用链急剧加长；
    -   并发问题：由此而导致奇怪问题次数大增，如网包或 message 丢失，同时 debug 变得比较困难的

-   多重数据拷贝：采用微服务后，除了把变更数据记录在本地数据库外，还对外广播，同时无形把网络流量拉升若干个数量级，同时相同或类似的数据拷贝存在无数个版本；

-   复杂性、封闭性增加，质量下降：服务的拆分很容易变成 premature optimization（一开始时就拆分而不是系统成熟时或对系统有成熟认识时拆分），做着做着，Json 和 REST 会变得越来越臃肿，理论上可以各自演进，实际却极大可能从一坨屎 💩 变成 一坨一坨的屎 💩💩💩；

-   所有的微服务拆分之后还要合成一个有机的整体，系统控制和设计不会因为拆分而消失，反而因为拆分而在整体上大大增加确定 `数据和系统正确性` 的难度 ⏤ distributed state/multiple data versions，统一的系统设计和实施尤其重要，系统如何正确控制和响应各种 failures，如何能能从失败中恢复到正确状态，必须在设计中事先考虑，这应该是分布式最大的技术难点 ⏤ [`Fallacies of distributed computing`](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)，经典的分布式设计或编码 8 种错误假设：

    -   网络是稳定的
    -   网络传输的延迟是零
    -   网络的带宽是无穷大
    -   网络是安全的
    -   网络的拓扑不会改变
    -   只有一个系统管理员
    -   传输数据的成本为零
    -   整个网络是同构的

谷歌认为测试必须在生产环境才算是真正的测试，我本人不是太热衷于 mocked 测试，而觉得 contract 更重要。强大的运维，监控，调试，恢复手段对微服务尤其重要，例如死信队列，流量重放，数据高可用，动态日志，容器化应用，等等。除了这些运维辅助手段，简洁的接口设计，编码质量，设计文档，是每一个好系统的特征。

> `系统集成依旧是所有复杂商业软件系统设计的难点和关键点`

## 微服务的小结

-   微服务不是新鲜事物，本质是分布式系统；
-   微服务架构只覆盖了软件基础架构，系统设计依旧要在整体上回答如何统一完成各种商业需求；
-   DevOps 必须先行；
-   复杂度加大，开发体验下降，对开发者要求提高；
-   复杂系统不易，复杂系统的微服务实施必须谨慎，相当谨慎；
-   以云原生为基础架构的微服务是技术发展的方向；

99% 的软件项目都是被“复杂度”杀死的，而这是微服务/分布式系统的死结（参考经典文章：[How Complex Systems Fail](http://web.mit.edu/2.75/resources/random/How%20Complex%20Systems%20Fail.pdf)）。

> `作为架构师，抛开各种技术和复杂表象，其工作核心就是: 对抗熵增（We want to fight entropy），办法就是 KISS`

![complexity](/images/arch/complexity.jpg#center)

---

软件开发是以最终产品为导向的，所以本质上我倾向把自己当作工程师，软件产品的制造者和拥有者，而非单纯的架构师或者程序员。

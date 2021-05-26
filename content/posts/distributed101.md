+++
title = "面向分布式系统工程师的分布式系统理论（译）"
date = 2020-05-10T18:18:02+08:00
readingTime = true
categories = ["系统设计 & 架构"]
tags = []
toc = true
+++

Distributed systems theory for the distributed systems engineer 简单明了的分布式入门读物

<!--more-->

原文：[<i class="fas fa-external-link-alt"></i>&nbsp;&nbsp; Distributed systems theory for the distributed systems engineer（2014）](http://the-paper-trail.org/blog/distributed-systems-theory-for-the-distributed-systems-engineer/)  
译者：[<i class="fas fa-external-link-alt"></i>&nbsp; 夏永锋](http://blog.xiayf.cn/2014/08/10/Distributed-systems-theory-for-the-distributed-systems-engineer/)

原文在 2018 年进行了更新，我在夏永锋译文基础上做了部分更改。

---

Gwen Shapira，曾今 的 Cloudera 工程师，现在引领 Kafka，在 [Twitter 上提的一个问题](https://twitter.com/gwenshap/status/497203248332165121) 引起了我的思考。

> 我需要提高我在分布式系统理论方面的熟知程度，我该怎么开始，有哪些推荐书籍？

如果是以前，我可能会回答“嗯，这里有篇 FLP 论文，这里有篇 Paxos 论文，这里还有篇拜占庭将军问题（the Byzantine generals）的论文...”，我会罗列一箩筐重要的材料，如果你一头扎进去，至少花费 6 个月的时间才能过一遍这些材料。然而我已逐渐明白推荐大量的理论性的论文通常恰恰是着手学习分布式系统理论的错误方式（除非你在做一个 PhD 项目）。论文通常比较深入难懂，需要认真地研习，通常还需要大量的时间投入（significant experience）来理清这些论文的重要贡献，以及在整个理论体系中的位置。要求工程师具备这样的专业水平又有多大的意义呢？

但是，很遗憾，对分布式系统理论方面的重大研究成果和思想进行概括、归纳、背景分析的‘导引’性质的优秀材料非常缺乏；特别是没有那种高屋建瓴的材料。对这块空白区域的思考让我想到了另一个有趣的问题：

<font color="orange">一个分布式系统工程师应该知道些什么分布式系统理论？</font>

在这种情况下，一知半解（a little theory）并不会是一件多危险的事情。因此我尝试整理一个列表，罗列出作为一个分布式系统工程师的我认为能够直接应用于我日常工作的基础理论。如果你认为我漏掉了一些东西，请联系我。

## 入门第一步

以下 4 篇材料出色地解释了构建分布式系统会遇到的一些挑战，共同概述了一系列抽象，但分布式系统工程师必须要解决的技术难题，为之后章节中更深入的研究做好准备。

-   [好玩又实在的分布式系统理论](http://book.mixu.net/distsys/)：是一本简短的书籍，其内容覆盖了分布式系统领域的一些基本议题，包括时间的作用及不同的复制策略。
-   [为分布式系统领域新人整理的笔记](http://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/)：不是对理论地讲述，而是做一个非常好的平衡，让你对其余材料的阅读能够落地。
-   [分布式系统研究综述报告](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.7628)：一篇经典的论文，解释了为什么不能将远程交互当作本地对象处理。
-   [关于分布式计算的若干谬论](http://en.wikipedia.org/wiki/Fallacies_of_Distributed_Computing)：分布式计算方面的 8 点谬论，提醒系统设计者可能会忘记的几类事情。

你需要知道分布式系统中的 <font color="orange">Safety</font> 和 <font color="orange">Liveness</font> 属性：

-   Safety 属性指的是不好的结果永远不会发生：系统永远不返回不一致的结果就是 Safety 属性，例如系统不会在同一时刻选出两个 leader；
-   Liveness 属性指的是好的结果最终会发生：系统最终一定会返回 API 调用的结果就是 Liveness 属性，例如系统保障每个磁盘写调用一定会处理完成。

## 失败和时间

分布式系统工程师需要面对的许多困难最终都可以归咎于两个底层的原因：

-   进程可能会失败
-   不存在一种好的方式来告知目前为止进程已经做了些什么

进程之间如何共享各自的时间信息？哪些失败的场景是能够检测到？什么算法和原语可能被正确地实现？这三个问题有着非常深层的联系。多数时候，我们会假设两个不同节点之间对对方的时间信息，或者对方时间过了多久是完全无知的。

你应该知道：

-   失败模式的层级（部分）：[崩溃停止->排除(omission)](http://www.cse.psu.edu/~gcao/teach/513-00/c7.pdf) -> [拜占庭容错](http://en.wikipedia.org/wiki/Byzantine_fault_tolerance)。在上层可能发生的问题在下层肯定可能发生，在下层不可能发生的问题在上层也肯定不可能发生。
-   在没有任何共享时钟的情况下如何判断在另一个事件之前是否产生了某事件。这意味着你需要理解 [Lamport 时钟](http://web.stanford.edu/class/cs240/readings/lamport.pdf) 及其一般化的 [向量时钟](http://en.wikipedia.org/wiki/Vector_clock)，也需要阅读一下 [这篇 Dynamo 论文](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)。
-   单个失败发生的可能性对于我们实现正确的分布式系统到底会有多大的影响（参考下面本人关于 FLP 的结论）
-   不同的时间模型：同步、部分同步和异步（参考 [这篇文章](https://groups.csail.mit.edu/tds/papers/Lynch/lncs90-asilomar.pdf)）
-   如何检测失败是一个需要回答的基础性问题，一个挑战是要对准确性和完整性做取舍；另一个挑战是 Safety 与 Liveness 的矛盾。这篇文章说明了失败检测是一个理论性的问题 [Chandra and Toueg’s ‘Unreliable Failure Detectors for Reliable Distributed Systems’](http://courses.csail.mit.edu/6.852/08/papers/CT96-JACM.pdf)。也有好几个简短的总结 - 我个人比较喜欢来自 [斯坦福大学的这个](http://www.scs.stanford.edu/14au-cs244b/labs/projects/song.pdf)。

## 容错的基本矛盾

一个系统，若要不降级而容忍某些错误的发生，就必须能够好像那些错误没有发生一样地运作。这通常意味着系统的这些部分必须能够冗余地工作，但是在非绝对必要情况下，做更多的工作会在性能和资源方面产生消耗。这是为系统添加容错带来的矛盾。

你应该知道：

-   确保单拷贝可串行化（single-copy serialisability）的仲裁技术（quorum）- 可阅读 [Skeen 的原始论文](https://ecommons.library.cornell.edu/bitstream/1813/6323/1/82-483.pdf)，但可能更建议阅读 [这个 Wikipedia 词条](<http://en.wikipedia.org/wiki/Quorum_(distributed_computing)>)
-   关于 [两阶段提交](http://the-paper-trail.org/blog/consensus-protocols-two-phase-commit/)、[三阶段提交](http://the-paper-trail.org/blog/consensus-protocols-three-phase-commit/) 和 [Paxos](http://the-paper-trail.org/blog/consensus-protocols-paxos/) 算法，以及为什么它们有不同的容错性质
-   最终一致性，及其他技术是如何以弱化对系统行为的保证为代价来尝试避免这种矛盾的，这篇 [Dynamo 论文](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)是一个很好的起点，同时 Pat Helland 的经典之作 [Life Beyond Transactions](http://www.ics.uci.edu/~cs223/papers/cidr07p15.pdf) 也是必读的

## 基本的原语

对于如何构建分布式系统，很少有大家一致认同的基础模块，但越来越多地在出现。你应该知道以下的问题是什么，以及在哪可以找到它们的解决方案：

-   leader 选举（leader election）：例如 [Bully 算法](http://en.wikipedia.org/wiki/Bully_algorithm)
-   一致的快照（consistent snapshotting）：例如 Chandy 和 Lamport 所写的 [经典论文](http://research.microsoft.com/en-us/um/people/lamport/pubs/chandy.pdf)
-   共识（consensus）：阅读上文提到的关于 2PC 和 Paxos 的博文
-   分布式状态机复制（distributed state machine replication）：看看 [Wikipedia](http://en.wikipedia.org/wiki/State_machine_replication) 就可以，但 [Lampson 的论文](http://research.microsoft.com/en-us/um/people/blampson/58-Consensus/Acrobat.pdf) 更权威，只是枯燥了点
-   广播（broadcast） - 一次性向多个节点上发布消息
    -   Atom 方式 - 你能否要么成功发布消息到所有节点，要么一个都没有？
    -   Gossip 方式：[Gossip 经典论文](http://bitsavers.informatik.uni-stuttgart.de/pdf/xerox/parc/techReports/CSL-89-1_Epidemic_Algorithms_for_Replicated_Database_Maintenance.pdf)
    -   Causal 因果排序组播方式：参考 [这篇论文](https://www.cs.cornell.edu/courses/cs614/2003sp/papers/BSS91.pdf)，也可欣赏 Birman 和 Cheriton 对此提出的 [质疑](https://www.cs.rice.edu/~alc/comp520/papers/Cheriton_Skeen.pdf)
-   链复制（chain replication）：把节点组织成一个虚拟的链表来保障写的一致性和顺序性
    -   [原始论文](http://www.cs.cornell.edu/home/rvr/papers/OSDI04.pdf)
    -   [一系列改进](https://www.usenix.org/legacy/event/usenix09/tech/full_papers/terrace/terrace.pdf)
    -   [实验报告](https://pdfs.semanticscholar.org/6b14/dd57eaf8122dbc29d08e50749661d4602e53.pdf)

## 基础结论

某些结论是需要内化于心的，以下是几个关键点（a flavour），当然可以更多：

-   如果进程之间可能丢失某些消息，那么不可能在实现存储一致性的同时能响应所有的请求。这就是 [CAP 定理](http://lpd.epfl.ch/sgilbert/pubs/BrewersConjecture-SigAct.pdf)。
-   共识机制不可能以这样一种方式实现 - 同时满足 a & b：
    -   a. 总是正确的
    -   b. 总是能终止的 - 在异步系统中，即使仅有一台机器失败（因 FLP 的 crash-\*stop 结果而导致）

我希望在 Papers We Love 洛杉矶座谈会上合理地解释了这个结论，参考[幻灯片的第一部分-进行证明之前](http://www.slideshare.net/HenryRobinson/pwl-nonotes) （提示：没有实际必要去理解如何证明）

-   一般而言，少于两轮消息交互是不可能达成共识（consensus）。
-   达成原子广播和达成共识一样难，准确的讲，如果能解决原子广播的问题，共识的问题也能解决，反之亦然，Chandra 和 Toueg 已经给出[证明](https://www.cs.utexas.edu/~lorenzo/corsi/cs380d/papers/p225-chandra.pdf)，你只要知道这个结论就好。

## 真实的系统

最重要的练习是重复地阅读新兴的、真实系统的描述，并尝试评价它们的设计决策，一遍又一遍地这样去做。一些建议：

-   Google:  
     [GFS](http://static.googleusercontent.com/media/research.google.com/en/us/archive/gfs-sosp2003.pdf)、[Spanner](http://static.googleusercontent.com/media/research.google.com/en/us/archive/spanner-osdi2012.pdf)、[F1](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/41344.pdf)、[Chubby](http://static.googleusercontent.com/media/research.google.com/en/us/archive/chubby-osdi06.pdf)、[BigTable](http://static.googleusercontent.com/media/research.google.com/en/us/archive/bigtable-osdi06.pdf)、[MillWheel](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/41378.pdf)、[Omega](http://eurosys2013.tudos.org/wp-content/uploads/2013/paper/Schwarzkopf.pdf)、[Dapper](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/36356.pdf)、[Paxos Made Live](http://www.cs.utexas.edu/users/lorenzo/corsi/cs380d/papers/paper2-1.pdf)、[The Tail At Scale](http://cacm.acm.org/magazines/2013/2/160173-the-tail-at-scale/abstract)。

-   Not Google:  
    [Dryad](http://research.microsoft.com/en-us/projects/dryad/eurosys07.pdf), [Cassandra](https://www.cs.cornell.edu/projects/ladis2009/papers/lakshman-ladis2009.pdf), [Ceph](http://ceph.com/papers/weil-ceph-osdi06.pdf), [RAMCloud](https://ramcloud.stanford.edu/wiki/display/ramcloud/RAMCloud+Papers), [HyperDex](http://hyperdex.org/papers/), [PNUTS](http://www.mpi-sws.org/~druschel/courses/ds/papers/cooper-pnuts.pdf)

---
layout:     post
title:      敏捷团队中的测试策略 (in progress)
subtitle:   
date:       2017-10-21 12:00:00
author:     DanteYu
tags:
    - TestStrategy
---

这篇文章主要总结了我对于敏捷团队中的测试策略的理解，主要来自于工作上的实践和思考。

### 测试策略的定义

先看下维基百科上关于[test strategy](https://en.wikipedia.org/wiki/Test_strategy)的定义

> A test strategy is an outline that describes the testing approach of the software development cycle. It is created to inform project managers, testers, and developers about some key issues of the testing process. This includes the testing objective, methods of testing new functions, total time and resources required for the project, and the testing environment.

> Test strategies describe how the product risks of the stakeholders are mitigated at the test-level, which types of testing are to be performed, and which entry and exit criteria apply. They are created based on development design documents. System design documents are primarily used and occasionally, conceptual design documents may be referred to. Design documents describe the functionality of the software to be enabled in the upcoming release. For every stage of development design, a corresponding test strategy should be created to test the new feature sets.

归纳上面的定义，我们可以得出测试策略的最终目的是通过定义项目会采用的测试活动，尽可能得暴露和消除产品缺陷，减轻产品风险。除此之外，由于软件开发时常伴有交付压力，测试的时间和资源都是无法保证甚至可能被压缩的，所以测试策略还会从最低成本揭露产品质量风险出发，选择出最合理的测试方法和测试过程。

### 测试策略的作用
基于上面的定义，可见测试策略解决了两个大问题：
1. “测什么”
2. “怎么测”

在详细点就是：
1. 确定了被测对象需要测试范围
2. 确定了会使用哪些测试技术来达到什么样的质量标准
3. 确定了项目的测试流程
4. 确定了每一种测试技术的具体使用方式，包括待使用的框架和工具等
5. 统一项目内使用的测试相关的术语

### 测试策略和测试计划的不同

要说不同，先看定义上的不同。

Test plan | Test strategy
------------ | -------------
测试策略详细描述了QA使用什么样的具体测试方法来达成测试目标，给测试流程和活动设置了标准，主要关注“测什么”和“怎么测”。 | 测试计划的主要目标是包含所有关于测试的信息，比如“为什么测”、测什么”、“什么时候测”、“怎么测”以及由“谁来测”。

由上可见，其实测试策略的内容已经被包含在测试计划里面了。测试策略定义应该做什么样的测试而测试计划包含所有关于测试策略该如何执行的信息，这些信息在测试计划里面被很好的组织起来。

一个公式可以进一步说明他们的关系 `test plan = test strategy + test logistics`。所以test strategy可以被看做是test plan的一部分。`Test logistics`是指测试环境设置以及人力资源等等。

在James Bach的博客[A Question About Test Strategy](http://www.satisfice.com/blog/archives/63)中，他把三者描述如下

> **Test Plan**: the set of ideas that guide a test project

> **Test Strategy**: the set of ideas that guide test design

> **Test Logistics**: the set of ideas that guide the application of resources to fulfill a test strategy

> I find these ideas to be a useful jumping off point. Here are some implications:

> The test plan is the sum of test strategy and test logistics.

> The test plan document does not necessarily contain a test plan. This is because many test plan documents are created by people who are following templates without understanding them, or writing things to please their bosses, without knowing how to fulfill their promises, or simply because it once was a genuine test plan but now is obsolete.

> Conversely, a genuine test plan is not necessarily documented. This is because new ideas may occur to you each day that change how you test. In my career, I have mostly operated without written test plans.

![test_strategy](https://github.com/DanteYu/DanteYu.github.io/blob/master/_posts/images/test_strategy.PNG?raw=true)

### 敏捷团队需要测试策略吗

让我们先来看下QA在敏捷项目中的主要工作，如下图所示
![iteration1](https://github.com/DanteYu/DanteYu.github.io/blob/master/_posts/images/iteration.png?raw=true)
那你可能问“咦，怎么没有测试策略相关内容呢”。其实，整个开发测试流程就体现了测试策略的内容。上图所示的迭代开发流程里面包含了"Automated Acceptance Tests" & “Story Testing” & “System Testing”这些测试活动，那么为什么项目需要这些测试活动？这些活动如何具体如何开展？分别在哪一个项目阶段进行？这些问题都属于测试策略的范畴，是由测试策略定义和落地的。

敏捷开发模型下，迭代式的开发具有周期短和交付压力大的特点，对于如何快速响应新需求，保障新需求的质量以及已实现需求的回归测试都将是测试的挑战。如果没有一个匹配项目上下文，合理规划了测试活动的测试策略，这些挑战就会持续困扰着团队，所以标题的答案是当然需要。测试策略在敏捷开发模型下，通过详细定义项目的测试活动，能够更加合理地利用测试资源和统一项目对测试的认知。

此外，测试策略也是敏捷项目质量保障体系中重要的一节。

### 敏捷团队需要测试计划吗

在传统瀑布开发模式下，测试计划test plan被认为是项目测试流程的关键部分，因为它指导着整个测试活动的开展，既然被认为是项目中的一个must have，于是有人会花很多时间很多精力去把测试计划给写好写完整。那么我们真的在敏捷开发模式的项目里需要一份测试计划吗？这真的能给项目质量带来价值吗？

敏捷宣言说过：
* 工作的软件 高于 详尽的文档
* 响应变化 高于 遵循计划
尽管右项有其价值，我们更重视左项的价值

在敏捷项目中，产品范围也就是发布内容在spring进行之前就被讨论，所以QA们对于测试对象和测试范围一直都是清楚的，不需要特别地花时间去写一个详细的文档来阐述。同样地，agile ceremony会让QA们清楚地知道测试对象是什么，范围是什么，重点是什么，测试环境是什么而不需要一个单独的文档来进行详细的描述。甚至，所有关于测试的信息还可以被记录被故事卡中。在开发进行编码实现功能的时候，QA们会进行测试用例设计以及自动化测试编写，因为时间的紧迫，QA除了这两项测试活动，再去写一个详细测试计划是不经济的且价值不大，这两项测试活动才是敏捷项目中价值最高的，况且随着迭代的进行，测试计划的维护还需要时间精力。

综上所述，在敏捷项目中因为时间紧迫和避免重复劳动，价值不高的测试计划不是一个must have，个人甚至认为也不是一个nice to have。但是这并不是代表我们不需要"planning"，而是不是需要"document planning"，"planning"的实施发生在迭代中的各类活动。

最终我们还是需要一个“计划”来指导迭代中的测试流程和方法，这就是测试策略是一个must have的原因。在敏捷项目中，“小而精”的测试策略比起“大而全”的测试计划而言，最大的优势就是测试策略定义的内容(“怎么测”)是不会轻易受影响改变的，并且在迭代中没有类似的重复活动。

### 什么时候着手制定测试策略

具体到项目里，测试策略推荐在项目刚启动的时候制定。测试策略需要在项目早期就制定下来，用来指导项目的测试活动和方法，从而确定需要的测试资源和保证质量体系的建立。但也不能在产品和技术都还没有确定的时候就制定，因为只有在产品需求范围，技术架构和交付计划大致确认的情况，测试策略才能更加准确，从而减少维护成本。

### 谁来主导测试策略的编写

因为测试策略会涉及到很多具体的测试技术和方法，也会要求制定人员具有对质量和测试非常深的理解，所以QA在敏捷团队中应该承担制定测试策略的主要责任，但是这并不意味“只有”QA来编写制定测试策略，测试策略的制定是一个团队活动，QA需要从不同角色收集到不同的信息

1. 从Business了解到产品愿景，产品发展蓝图和业务流程 - 涉及需求分析和确定测试目标等活动
2. 从Project Manager了解到交付计划，交付范围 - 涉及确定测试类型、测试方法、测试阶段和测试重点等活动
3. 从Tech Lead了解到技术框架 - 涉及确定集成测试、自动化测试语言、工具和框架选型等活动

从多方收集信息能够保证业务、技术和质量三者对齐，避免误解和冲突，共同发挥最大的作用。

当测试策略制定完成以后，还需要给项目团队进行讲解，确保团队所有相关人员对项目中的测试活动和测试方法有着一致的理解,这样才能保证实施阶段的顺利。

### 在敏捷团队中制定测试策略的流程

接下来会涉及到QA制定测试策略的具体活动及流程。总的来说，大体流程可以如下
1. 前期项目信息收集
2. 确立质量目标
3. 确定测试类型
4. 确定测试阶段
5. 确定测试度量

##### 前期项目信息收集

上面提到了QA可以从不同角色收集到不同的信息，除此之外，使用启发式测试计划语境模型 Heuristic Test Planning Context Model和启发式测试策略模型 Heuristic Test Strategy Model也是一个有效的渠道。具体如何使用这两个模型，请参考[htsm](https://danteyu.github.io/2017/06/21/htsm/)和[htcpm](https://danteyu.github.io/2017/07/13/htpcm/)

只有收集到足够的项目信息，才能帮助我们少走弯路，制定出适合项目和团队的测试策略。

##### 确定质量目标 质量模型

在具体思考“怎么测”之前，我们需要确定项目的质量目标。这个质量目标会对齐项目所有相关人员对于项目质量的理解和期望。那一般来讲，有哪些候选人




此外，还有团队确定了测试策略目标宣言
```
“To Constantly Deliver Working Software that Meets Customer’s Requirements”
by means of
“Providing Fast Feedback”
and
“Defect Prevention, rather than Defect Detection”
```

##### 确定测试类型

有了质量目标，接下来我们要考虑要怎么达成这个目标了！也就是说，我们应该有哪些测试活动来覆盖每一个目标，每个测试活动又有哪些不同的测试类型组成？




测试四象限
自动化测试策略 - 金字塔
##### 确定测试框架
##### 确定测试阶段


角色定义职责
测试交付物
##### 测试度量
质量度量？ 敏捷度量？

### 测试策略的持续改进

一旦测试策略被确定，一般来讲不会经常变化，因为测试策略设置了一些测试标准。如果测试标准频繁地变动，这会让具体计划的执行变得困难，同时带来更多的资源浪费，最终影响了项目的质量。

在项目中我们会经常遇到“变化”：比如说
* feature scope的变化
* 具体功能细节的变化
* 测试重点的变化
* 根据risk assessment来调整的变化

这些变化对测试的影响是被测对象的范围以及项目资源的调配。对于项目的质量目标、测试类型、测试阶段影响不大。所以，测试策略一旦被制定出来，变化不会太大。

不过这不代表测试策略的一成不变和缺乏改进，QA需要在每个迭代中观察测试策略实施的效果来决定当前的测试类型和实施手段是否满足项目需求。比如当项目集成测试(API Testing)经常fail，且协调工作耗时费力，QA可以考虑采用契约测试来代替现有的集成测试，提高整个项目组的生产率和质量；比如发现Internet Edge突然用户量增多且有关于Internet Edge的production bug被raise，QA需要把Internet Edge加到兼容性测试中，并且设置相关的测试环境。

### 写在最后

上面详细阐述了我如何理解敏捷项目中的测试策略以及制定测试策略的具体步骤。由于IT项目的多样性和复杂性，这个总结不可能适用于有着不同上下文的项目，因地制宜地制定测试策略才能保证测试策略在项目中的可用性和合理性。

### 参考资料

* [从测试策略到测试架构](http://insights.thoughtworks.cn/from-strategy-to-architecture/)
* [究竟什么是软件测试策略](https://mp.weixin.qq.com/s?__biz=MjM5ODczMDc1Mw==&mid=2651843309&idx=1&sn=035fc3dbd0ec09d9af5273448f26aa6a&chksm=bd3d07838a4a8e95da90e3d904a714cb746bfd9b5561b78eab0b945dbd5199a8972834075755&scene=0&key=bca8d81f29a4d80e9a99a34999b3106c89c2d69117c01bd7d890e24c8e57ceb4454b4ad2325a612758ee4c17a247a4b93079841f50427a4ad63f3b5a42bcf7c1a7dd62137ce47d6cc6a04657bf3c7b10&ascene=0&uin=MjMyODY4NTU1&devicetype=iMac+MacBookAir6,2+OSX+OSX+10.12.2+build(16C67)&version=12010310&nettype=WIFI&fontScale=100&pass_ticket=xK4r8BIZZrhw3NxB2LSjkI+mRwPZZvhse+yJPq6jtz0=)
* [agile-test-plan-do-we-really-need-one](https://www.testingexcellence.com/agile-test-plan-do-we-really-need-one/)
* [A Question About Test Strategy](http://www.satisfice.com/blog/archives/63)

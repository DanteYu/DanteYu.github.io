---
layout:     post
title:      敏捷团队中的测试策略 (in progress)
subtitle:   
date:       2017-10-21 12:00:00
author:     DanteYu
tags:
    - TestStrategy
---

这篇文章主要总结了我对于敏捷团队中的测试策略的理解。

### 敏捷团队需要测试策略吗

让我们先来看下QA在敏捷项目中的主要工作，如下图所示
![iteration1](https://github.com/DanteYu/DanteYu.github.io/blob/master/_posts/images/iteration.png?raw=true)
那你可能问“咦，怎么没有测试策略相关内容呢”。其实，整个开发测试流程就体现了测试策略的内容。上图所示的迭代开发流程里面包含了"Automated Acceptance Tests" & “Story Testing” & “System Testing”这些测试活动，那么为什么项目需要这些测试活动？这些活动如何具体如何开展？分别在哪一个项目阶段进行？这些问题都属于测试策略的范畴，是由测试策略定义和落地的。

敏捷开发模型下，迭代式的开发具有周期短和交付压力大的特点，对于如何快速响应新需求，保障新需求的质量以及已实现需求的回归测试都将是测试的挑战。如果没有一个匹配项目上下文，合理规划了测试活动的测试策略，这些挑战就会持续困扰着团队，所以标题的答案是当然需要。测试策略在敏捷开发模型下，通过详细定义项目的测试活动，能够更加合理地利用测试资源和统一项目对测试的认知。

此外，测试策略也是敏捷项目质量保障体系中重要的一节。

### 测试策略的定义

先看下维基百科上关于[test strategy](https://en.wikipedia.org/wiki/Test_strategy)的定义

> A test strategy is an outline that describes the testing approach of the software development cycle. It is created to inform project managers, testers, and developers about some key issues of the testing process. This includes the testing objective, methods of testing new functions, total time and resources required for the project, and the testing environment.

> Test strategies describe how the product risks of the stakeholders are mitigated at the test-level, which types of testing are to be performed, and which entry and exit criteria apply. They are created based on development design documents. System design documents are primarily used and occasionally, conceptual design documents may be referred to. Design documents describe the functionality of the software to be enabled in the upcoming release. For every stage of development design, a corresponding test strategy should be created to test the new feature sets.

归纳上面的定义，我们可以得出测试策略的最终目的是通过定义项目会采用的测试活动，尽可能得暴露和消除产品缺陷，减轻产品风险。除此之外，由于软件开发时常伴有交付压力，测试的时间和资源都是无法保证甚至可能被压缩的，所以测试策略还会从最低的成本揭露产品质量风险出发选择出最合理的测试方法和测试过程。

### 测试策略的作用

基于上面的定义，可见测试策略解决了两个大问题：
1. “测试什么”
2. “怎么测”

在详细点就是：
1. 确定了被测对象需要测试范围
2. 确定了会使用哪些测试技术来达到什么样的质量标准
3. 确定了每一种测试技术的具体使用方式，包括待使用的框架和工具等
4. 确定了项目的测试流程
5. 统一项目内使用的测试相关的术语

### 测试策略和测试计划的不同

一个公式可以说明 `test plan = test strategy + test logistics` 所以test strategy可以被看做是test plan的一部分。Test logistics是指测试环境设置以及人力资源等等。

定义上来说，测试计划定义了测试范围、目标、强调了我们需要花费多少的测试。测试策略则是强调测试技术的设计和具体实践。



### 什么时候着手制定测试策略
具体到项目里，测试策略推荐在项目初期，还没有具体开发测试之前这个阶段来制定。这个阶段的特点是比如在项目刚启动的时候或是还处于探索提议阶段

在产品需求范围和交付计划大致确认

应该在项目生命周期最早的阶段进行开发，。

### 在敏捷团队中制定测试策略的流程
确定测试目标 质量目标 质量模型
测试类型
测试框架
测试阶段

### 测试策略的持续改进

迭代中调整
  scope的变化
  测试重点的变化

根据risk assessment来调整


### 参考资料

* [从测试策略到测试架构](http://insights.thoughtworks.cn/from-strategy-to-architecture/)
* [究竟什么是软件测试策略](https://mp.weixin.qq.com/s?__biz=MjM5ODczMDc1Mw==&mid=2651843309&idx=1&sn=035fc3dbd0ec09d9af5273448f26aa6a&chksm=bd3d07838a4a8e95da90e3d904a714cb746bfd9b5561b78eab0b945dbd5199a8972834075755&scene=0&key=bca8d81f29a4d80e9a99a34999b3106c89c2d69117c01bd7d890e24c8e57ceb4454b4ad2325a612758ee4c17a247a4b93079841f50427a4ad63f3b5a42bcf7c1a7dd62137ce47d6cc6a04657bf3c7b10&ascene=0&uin=MjMyODY4NTU1&devicetype=iMac+MacBookAir6,2+OSX+OSX+10.12.2+build(16C67)&version=12010310&nettype=WIFI&fontScale=100&pass_ticket=xK4r8BIZZrhw3NxB2LSjkI+mRwPZZvhse+yJPq6jtz0=)

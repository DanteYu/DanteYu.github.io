---
layout:     post
title:      敏捷团队中的测试策略
subtitle:   
date:       2017-10-21 12:00:00
author:     DanteYu
tags:
    - TestStrategy
---

这篇文章主要总结了我对于敏捷项目中总体测试策略的理解，主要来自于工作上的实践和思考。

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
5. 探索测试的深度和广度
6. 探索测试的重点和难点
7. 统一项目内使用的测试相关的术语
8. 确定了质量度量

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
4. 确定测试工具和框架
5. 确定测试阶段
6. 确定测试度量
7. 持续改进和风险分析

#### 前期项目信息收集

上面提到了QA可以从不同角色收集到不同的信息，除此之外，使用启发式测试计划语境模型 Heuristic Test Planning Context Model和启发式测试策略模型 Heuristic Test Strategy Model也是一个有效的渠道。具体如何使用这两个模型，请参考[htsm](https://danteyu.github.io/2017/06/21/htsm/)和[htcpm](https://danteyu.github.io/2017/07/13/htpcm/)。

通过分析[htsm](https://danteyu.github.io/2017/06/21/htsm/)中“项目环境”和“产品元素”的关键词和启发式问题，QA可以了解关于产品和项目的各类信息来帮助制定测试策略。[htcpm](https://danteyu.github.io/2017/07/13/htpcm/)也提供了大量的关键词来启发QA去分析了解产品需求、项目环境、测试小组和测试资源。

可以收集的信息大致可分类如下
1. 产品信息
  * 商业目标
  * 产品愿景  
  * 相关利益者
2. 业务信息
  * 业务流程
  * 业务范围
  * 业务数据
3. 技术信息
  * 技术架构
  * 技术栈
4. 交付信息
  * 交付计划
  * 开发流程
  * 部署架构
  * 持续交付
  * 线上监控
5. 团队信息
  * 组织结构
  * 测试资源

只有从不同的维度收集到足够的项目信息并且很好的理解这些信息对项目质量和测试活动的影响，才能帮助我们少走弯路，制定出适合项目和团队的测试策略。

#### 确定质量目标

在具体思考“怎么测”之前，我们需要确定项目的质量目标。这个质量目标会对齐项目所有相关人员对于项目质量的理解和期望，明确质量范围也就是测试策略会覆盖的范围。同时，质量目标也要与产品目标对齐，因为质量的最终目的也是保证产品的成功。根据产品在不同阶段都有不一样的目标，质量目标有会随之变化。

那质量目标如何设定？主要是参考软件质量特征列表和软件质量模型，建立符合项目上下文的产品质量模型，从而确定项目质量目标

要建立项目产品的质量模型首先就需要先知道一个软件产品的质量属性或特征分别有什么，对应的质量模型是什么样的。幸运的是现在已经有很多可以参考的模型了
* [软件质量特征列表](http://thetesteye.com/posters/TheTestEye_SoftwareQualityCharacteristics.pdf)
* [HTSM的Quality Criteria Categories](http://www.satisfice.com/tools/htsm.pdf)
* [ISO/IEC_9126](https://en.wikipedia.org/wiki/ISO/IEC_9126)
* [ISO/IEC_25010](https://blog.codacy.com/enterprise-software-a-summary-of-iso-25010-software-quality-model-7100575d6f6) [中文解析](https://mp.weixin.qq.com/s/3Y6CvG39EdeGFTlHWWgqLg)
* [其他软件质量模型](http://www.cnblogs.com/gaochundong/p/software_quality_models.html)

ISO/IEC_25010的质量模型大致如下：
![iso25010](https://github.com/DanteYu/DanteYu.github.io/blob/master/_posts/images/iso25010.png?raw=true)

从上面的质量特征列表或是模型可以看出，一个软件产品的质量由多个质量特征或标准组成，每个质量特征又可以进一步分解为子特征。通过这些已有的质量模型来启发哪些质量特征是对项目产品重要的，哪些质量标准适用于本项目产品，最后得出符合项目上下文的产品质量模型。

比如 我们创建的产品质量模型可以包含以下粗粒度特征：

1. 功能性
  * 完成度
  * 精准度
  * 互用性
  * 效率
  * 并发
2. 性能
  * 资源利用率
  * 吞吐量
  * 持久力
3. 安全
  * 认证
  * 授权
  * 隐私
4. 兼容性
  * 应用兼容
  * OS兼容
  * 硬件兼容
  * 向后兼容
5. 可用性
  * 易学性
  * 易操作性
  * 可达性
6. 可靠性
  * 稳定性
  * 健壮性
  * 可恢复性
  * 错误处理
  * 数据完整性
7. 可维护性
  * 可扩展性
  * 修复
  * 构建
8. 可移植性
  * 重用
9. 可安装性
  * 配置
  * 升级卸载

上面的质量模型定义了产品质量特征和标准，而这些特征和标准就是我们应该完成的目标！除了上面说到的质量模型，一些具体的metrics也可以被引入来保证项目质量，成为项目质量目标的一部分。举个例子

* 功能性
  * 验收条件(AC)通过率100%
  * UI自动化测试覆盖率30%
  * API自动化测试覆盖率90%
  * 单元测试覆盖率90%
  * 没有high severity bug
* 性能
  * 吞吐量定义
  * 资源利用率定义
  * 响应时间定义
* 安全
  * [OWASP TOP TEN](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* 兼容性
  * 浏览器支持
  * OS支持
* 可靠性
  * 线上bug响应时间
  * 灾难恢复时间
* 可安装性
  * 升级时间
  * 卸载时间
* 内部质量
  * 代码复杂度
  * 重复代码
  * checkstyle
  * 包耦合指数
  * 过多的注释
  * 未使用的代码
  * 逻辑错误
  * code convention
  * Runtime error

上面提到的标准都是可以通过集成到持续集成流水线的质量工具或测试框架来实现。

此外，还有团队也会使用测试策略目标宣言来打造团队文化。

```
“To Constantly Deliver Working Software that Meets Customer’s Requirements”
by means of
“Providing Fast Feedback”
and
“Defect Prevention, rather than Defect Detection”
```
#### 确定测试类型

有了质量目标，接下来我们要考虑要怎么达成这个目标了！也就是说，我们应该有哪些测试类型来覆盖每一个质量目标？

测试类型按照不同维度可以有很多种分类，比如说

1. 基于是否考虑软件内部结构和实现
  * 白盒测试
  * 黑盒测试
  * 灰盒测试
2. 基于是否执行程序
  * 静态测试
  * 动态测试
3. 基于测试过程
* 单元测试
* 集成测试
* 冒烟测试
* 功能测试(系统测试)
  * 探索式测试
  * 场景测试
  * 流测试
  * 域测试
* 兼容性测试
* 回归测试
* 验收测试
* 性能测试
  * 压力测试
  * 负载测试
* 安全测试

当然，上面只是列出了一部分。

此外，HTSM中的[Test Techniques](https://danteyu.github.io/2017/06/21/htsm/)提供了九种通用的九种测试技术来启发对可应用的测试类型的思考。敏捷测试四象限也提供了敏捷项目可以参考的测试类型,这个测试技术分类系统可以帮助我们快速定位某测试类型在软件开发中的位置，根据项目产品情况选择合适的测试类型。
![agile testing pyramid](http://www.agilebuddha.com/wp-content/uploads/2015/03/Agil-Testing-Quadrants.png)

就以我们上面假设的质量目标为例，我们来看看可以应用哪些测试类型

质量目标 | 测试类型
------------ | -------------
功能性 | 功能测试、集成测试、探索式测试、用户验收测试、流测试、回归测试
性能 | 压力测试、负载测试
安全 | 渗透测试、威胁建模
兼容性 | 兼容性测试
可用性 | 用户测试、Alpha测试、Beta测试
可靠性 | 风险测试、场景测试、域测试(domain testing)、流测试、压力测试
可维护性 | 单元测试
可移植性 | 专项测试
可安装性 | 专项测试

值得注意的是，并不是每一个项目都需要覆盖上面所列出的测试类型。我们应该根据产品的目标和特点以及其他实际情况选择与之对应的测试覆盖类型，也就是说，不同项目根据测试类型的不同，测试广度是不一样的。同事，根据测试的难点和重点，测试深度也是不同的。所以，一切都要基于项目的上下文来思考和制定。

##### 对于自动化测试单独的策略
自动化测试金字塔用来指导敏捷项目中自动化测试的策略。
- 金字塔底层是自动化测试的基础，主要包含单元测试和组件测试
- 金字塔中层是面向业务的自动化测试。调用产品提供的编程接口
- 金字塔顶层是少量的基于图形界面的自动化测试
- 金字塔上方是手工测试
![ideal automated testing pyramid](https://watirmelon.files.wordpress.com/2012/01/idealautomatedtestingpyramid.png)

根据金字塔理论，项目应该在底层的单元测试和集成测试(API测试)投入更多的精力。

自动化测试项目会被集成在持续集成流水线里面，由上游项目自动触发。为了减少持续集成的反馈时间，一个普遍的做法是把庞大的自动化测试套件集依据重要性优先级放在不同的流水线里面，可以被上游项目触发也可以定时触发，下面描述了一个比较好的实践：
1. 构建部署流水线，每个构建都要执行的测试，上游项目触发 --> 单元测试、接口测试、冒烟测试
2. 单独的测试项目，每日最新稳定版本要执行的测试，定时触发 --> 部分功能性回归测试、性能测试
3. 发布候选流水线，每个发布候选构建要执行的测试，定时触发 --> 全部功能性回归测试、性能测试

#### 确定测试框架和工具

确定测试类型之后，我们就知道了整个项目的测试活动会有哪些。对于某些测试类型，特别是自动化相关的测试，我们需要对应的测试工具或是框架来支撑我们的测试。所以我们需要对每一个测试类型都去思考下如何进行测试，是否需要相应的测试工具和框架的支持。

拿一个web程序来举例

1. 兼容性测试工具
  * browserstack
  * 指定的浏览器版本
2. 单元测试框架
  * Junit
  * pytest
  * Jest
  * Mocha
  * jasmine
3. mockup工具
  * wiremock
  * moco
  * mockito
4. 测试数据工具
  * faker
  * Lorem Ipsum
5. 集成测试框架
  * Rest assured
  * supertest
  * chakram
  * Pact
6. 集成测试工具
  * Postman
  * Swagger
6. UI功能测试框架
  * webdriver
  * webdriverio
  * Protractor
  * codeceptjs

#### 确定测试阶段

这个环节我们需要确定在项目开发生命周期的每个阶段做什么样的测试，使得测试类型与项目的开发流程充分结合，这样就能最大发挥所有测试活动的效果来达到我们的质量目标。因此，我们可以做出类似下面这样的表格来表现每个阶段的测试类型及其实施方法。

SDLC | 测试类型 | 方法 | 框架/工具 | 策略
------------ | -------------  | ------------- | ------------- |
Code | 单元测试 | 自动化 | Junit, pytest, mocha, jasmine | 采用TDD，覆盖率检查，QA review UT，每次构建在CI执行
Test | 冒烟测试 | 手工/自动化 | webdriver | 针对每次build在CI执行地自动化冒烟测试，在shoulder check环节的冒烟测试，与Build Verification Testing类似，每次build都要通过的测试
Test | 功能测试 (系统测试) | 手工/自动化 | webdriver | 故事卡测试 - 探索式测试，故事卡测试 - 场景验收测试，自动化UI测试，自动化API测试，自动化测试在CI流水线执行，自动化测试也可以单独在CI执行
Test | 集成测试 | 自动化 | Rest assured，pact | 覆盖依赖服务的测试，可以使用mock
Test | 性能测试 | 自动化 | locust，gatling | 压力测试，负载测试
Test | 兼容性测试 | 手工/自动化 | Browserstack | 确定产品支持的主流平台和浏览器
Test | 安全测试 | 自动化 | HP Fortify | 渗透测试
Test | 回归测试 | 手工/自动化 |故事卡缺陷的回归测试，AC的回归测试，相关代码配置改动引起的回归测试，缺陷卡回归测试，上线前的回归测试 - Candidate testing， PVT
UAT | 用户验收测试 | 手工 | | 每个故事卡QA测试结束后，需要客户进行在UAT环境进行测试。只有当UAT通过了，才表明这个故事卡done，QA需要在此环节和business进行协调

至此，测试策略解决的两个问题“测什么”和“怎么测”都可以找到大致答案了。

#### 测试度量

那如何衡量测试策略的有效性呢？质量度量可以告诉我们产品的质量情况和用户满意度，从而反应出测试策略是否有效和高效。

软件质量的度量没有什么最佳实践，也没有最准确和科学的度量，有的只是项目上积累的成功经验；但是这些成功经验又由于项目差异化太大而变得复用性很差。所以根据项目的上下文，我们需要制定出自己的质量度量标准。结合本文敏捷项目的背景，我们可以大致使用下面常用的度量：
* 缺陷统计度量
  * 解决率
  * 解决时间
  * 分布
  * 触发原因
  * 严重性
  * 优先级
  * 数量变化趋势
* 覆盖率
  * 需求覆盖率
  * 故事卡AC覆盖率
  * 代码覆盖率
    * 函数覆盖数量
    * 运行使用到的功能覆盖数量
    * 条件覆盖数量
    * path覆盖数量
  * 自动化测试覆盖率
    * 运行通过的测试比率
      * 功能稳定程度
    * 不同开发阶段的比率
* 测试执行
  * 测试执行率
  * 测试执行通过率
* 故事卡返工数量及比例
* 测试过的系统浏览器数量
* PO验收反馈

同时，实例化的质量目标也是很好的项目质量的工具。对于质量模型，我们可以看下每一个质量元素我们是否做到；对于具体的指标metrics，要随时监控反馈。

#### 持续改进和风险分析

一旦测试策略被确定，一般来讲不会经常变化，因为测试策略设置了一些测试标准。如果测试标准频繁地变动，这会让具体计划的执行变得困难，同时带来更多的资源浪费，最终影响了项目的质量。

在项目中我们会经常遇到“变化”：比如说
* feature scope的变化
* 具体功能细节的变化
* 测试重点的变化
* 根据risk assessment来调整的变化

这些变化对测试的影响是被测对象的范围以及项目资源的调配。对于项目的质量目标、测试类型、测试阶段影响不大。所以，测试策略一旦被制定出来，变化不会太大。

不过这不代表测试策略的一成不变和缺乏改进，QA需要在每个迭代中观察测试策略实施的效果来决定当前的测试类型和实施手段是否满足项目需求。比如当项目集成测试(API Testing)经常fail，且协调工作耗时费力，QA可以考虑采用契约测试来代替现有的集成测试，提高整个项目组的生产率和质量；比如发现Internet Edge突然用户量增多且有关于Internet Edge的production bug被raise，QA需要把Internet Edge加到兼容性测试中，并且设置相关的测试环境；比如测试进度落后，交付压力增大，QA需要及时调整测试范围和测试重点，进行风险分析。

在实际项目中，往往会出现各种各样的问题来阻碍测试策略的顺利落地和执行。这些问题有些是测试策略自身的问题，但也有项目带来的问题。这时候，风险分析显得格外重要。

对于测试策略的风险分析，这个是应该贯穿整个测试策略制定周期里面的，我们需要通过项目风险清单提前识别项目中可能阻塞测试的风险，并通过风险优先级(风险暴露的概率*风险暴露的损失)来评估风险，最终基于风险调整测试策略，把测试重点放到风险高优先级高的地方。

### 其他影响质量的因素
测试策略是影响质量的重要因素，但不是全部，下面列出了部分会影响质量的因素作为参考，在此文中不会展开来讲
1. 风险控制
2. 流程改进
3. 敏捷实践
4. 团队组成
5. 项目状况

### 写在最后

上面详细阐述了我如何理解敏捷项目中的测试策略以及制定测试策略的具体步骤。由于IT项目的多样性和复杂性，这个总结不可能适用于有着不同上下文的项目，因地制宜地制定测试策略才能保证测试策略在项目中的可用性和合理性。

### 参考资料

* [从测试策略到测试架构](http://insights.thoughtworks.cn/from-strategy-to-architecture/)
* [究竟什么是软件测试策略](https://mp.weixin.qq.com/s?__biz=MjM5ODczMDc1Mw==&mid=2651843309&idx=1&sn=035fc3dbd0ec09d9af5273448f26aa6a&chksm=bd3d07838a4a8e95da90e3d904a714cb746bfd9b5561b78eab0b945dbd5199a8972834075755&scene=0&key=bca8d81f29a4d80e9a99a34999b3106c89c2d69117c01bd7d890e24c8e57ceb4454b4ad2325a612758ee4c17a247a4b93079841f50427a4ad63f3b5a42bcf7c1a7dd62137ce47d6cc6a04657bf3c7b10&ascene=0&uin=MjMyODY4NTU1&devicetype=iMac+MacBookAir6,2+OSX+OSX+10.12.2+build(16C67)&version=12010310&nettype=WIFI&fontScale=100&pass_ticket=xK4r8BIZZrhw3NxB2LSjkI+mRwPZZvhse+yJPq6jtz0=)
* [agile-test-plan-do-we-really-need-one](https://www.testingexcellence.com/agile-test-plan-do-we-really-need-one/)
* [A Question About Test Strategy](http://www.satisfice.com/blog/archives/63)
* [软件质量模型](http://www.cnblogs.com/gaochundong/p/software_quality_models.html)
* [软件质量度量](http://download.microsoft.com/download/3/3/6/3364ea20-6347-462e-a264-f356fa726833/SDM312-zhangshuang.pdf)

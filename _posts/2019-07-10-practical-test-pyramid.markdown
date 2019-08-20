---
layout:     post
title:      实用的测试金字塔
subtitle:   
date:       2019-07-10 12:00:00
author:     DanteYu
tags:
    - TestStrategy
---
“测试金字塔”[Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html)是一个提出了有段时间的概念，它是自动化测试策略的理想模型，告诉了我们从成本的角度，我们应该如何组织我们不同类型的测试，比如测试的粒度和数量。最近才看到今年初新出了一篇“实用的测试金字塔”[The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)，这篇文章重温了金字塔的概念，结合近几年测试的发展，举了很多基于实战的经验和例子。这篇文章记录我的读后纪要。

#### 测试金字塔

测试金字塔在现在看起来可能会过于简单，在命名或概念上有些争论。但是测试金字塔还是可以成为你开始构建测试套件的参考原则，我们理解测试金字塔主要从以下两方面入手：
1. 不同的测试有不同的粒度
2. 越high level的测试应该写得越少

不用太过于纠结测试类型的名称，只要测试类型名称在项目组达成共识就可以。

##### 单元测试
如果是函数式语言，单元测试的范围就是函数；如果是面向对象语言，单元测试的范围可以从单一方法到整个类。基本思路都是使用不同的参数去验证返回值。
单元测试可以分为两类：
1. Solitary unit test - 使用`Test Double`(mocks and stubs)去代替所有测试对象的collaborators(被待测对象调用的类)。这样可以避免复杂的setup和一些副作用，排除由测试对象的环境可能引起的问题
2. Sociable unit test - 只有当collaborators很慢或者有副作用时才使用`Test Double`，比如网络链接或是数据库读取

至于选择哪一类单元测试，取决于你的项目情况。

`Mocks`和`Stubs`是`Test Double`的两种类型。`Test Double`并不是只有单元测试才能使用。

单元测试至少应该测试所有的public方法。private方法因为不能直接调用所以不用测试，protected方法可以通过测试其他public方法来测试。不需要测试所有的方法，不然太过了。所以一个原则就是：我们需要保证所有重要的代码路径需要被测试覆盖。

一个好的测试结构可以如下：
1. Set up the test data
2. Call your method under test
3. Assert that the expected results are returned

可以理解为`Arrange, Act, Assert`或是`Given, When, Then`。

##### End to End Test

基于cost-value ratio, End to End Test要控制在一个比较小的量级，能够覆盖代表产品最核心价值的user journey。那些组成user journey的steps就能够被自动化。

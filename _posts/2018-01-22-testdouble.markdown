---
layout:     post
title:      Test Double总结 (in progress)
subtitle:   
date:       2018-01-22 12:00:00
author:     DanteYu
tags:
    - TestDouble
---

### Test Double
第一次了解`Test Double`是在Martin Fowler的文章[Test Double](https://martinfowler.com/bliki/TestDouble.html)中，Gerard Meszaros提出了这个概念。虽然是06年的文章了，但里面的概念并不过时。这篇文章提到`Test Double`只是一个通用的词，代表为了达到测试目的并且减少被测试对象的依赖，使用“替身”代替一个真实的依赖对象，从而保证了测试的速度和稳定性。

在项目中，我们时常会遇到由于待测系统依赖组件无法工作而造成的测试阻碍，这是严重影响项目交付的风险之一，而`Test Double`就是规避这个风险的手段。在测试过程中，我们使用`Test Double`替代真实的依赖组件去和待测系统进行交互，`Test Double`不必和真实的依赖组件的实现一模一样，比如不用去实现依赖组件复杂的内部逻辑等，我们只需要在满足测试需求范围内，确保对于待测系统来说`Test Double`提供的API是和依赖组件提供的一样的，API是怎么实现的在这个上下文就显得不重要了。基于这个特点，`Test Double`多用于自动化测试比如单元测试和集成测试。

那么`Test Double`就是万能的吗？显然不是，毕竟我们是使用的是替代品而不是真实产品环境的配置，所以我们需要至少有一个测试去验证使用真实依赖对象的产品。此外，要时刻注意我们是使用`Test Double`去代替待测系统的依赖对象而不是直接去代替待测系统的部分功能，不然我们就在测试一个“错误”的产品。

`Test Double`可以进一步细化为：
* Test Stub
* Test Spy
* Mock Object
* Fake Object
* Dummy Object

它们各自的定义和区别是什么呢？这篇文章会解答这个问题。

#### Test Stub - you can define answers to me; I'll respond the same
`Test Stub`是指一个完全代替待测系统依赖组件的对象，这个对象按照我们设计的输出与待测系统进行交互，可以理解是在待测系统内部打的一个桩。这个桩既不会与测试用例(代码)交互，也不会在待测系统内部进行验证。`Test Stub`常用于响应待测系统的请求，然后返回特定的值。接下来，这个值会对待测系统产生影响，然后我们就在测试用例里面去验证这个影响。

`Test Stub`的实现方式一般有两种：
1. `Hard-Coded Test Stub` - 会返回固定response的Test Stub
2. `Configurable Test Stub` - 会根据测试需求返回相应response的Test Stub，可配置化

当我们遇到下面场景时，`Test Stub`就可以派上用场
* 依赖组件无法使用，影响测试结果
* 依赖组件运行太慢，影响测试速度
* 成为`Responder`响应者，当需要给待测系统注入特定数据，从而对待测系统产生影响
* 成为`Saboteur`破坏者，当需要给待测系统注入无效数据，从而对待测系统产生异常影响，观察待测系统如何处理错误情况

#### Mock Object - you can set your expectation on me
`Mock Object`是指一个完全代替待测系统依赖组件，并且用于验证待测系统输出的对象。这个对象接受待测系统的输出，进行处理并且这个输出进行验证，一旦验证通过也会返回值给待测系统。`Mock Object`主要用于接受待测系统的输出，然后进行验证。

`Mock Object`一个重要的特点是它可以对无法在待测系统上直接被观察到的行为或输出进行验证。无法观察到的系统行为或输出可以是数据插入数据库，可以是数据写入文件，也可以是对其他组件的调用。以数据库类型`Mock Object`举例，这个`Mock`的数据库会去接受待测系统发过来的数据，并且对这个数据进行验证，一旦验证通过就会对数据进行处理(插入或更新操作)，然后测试代码会去验证插入是否成功。

#### Fake Object - you can have me with limited capabilities
`Fake Object`是指一个轻量级的完全代替待测系统依赖组件的对象，采用更加简单的方法实现依赖组件的功能。`Fake Object`可以是一个“fake DB”比如简单的内存数据库来代替真实的重量级的数据库，也可以是一个“fake web service”比如创建一个简单的web service来返回指定的response。

`Fake Object`和`Test Stub`很类似，都是依赖组件的代替，区别就在于这个“轻量级”的定义。“轻量级”是指`Fake Object`仅仅提供和依赖组件一样的功能接口保证待测系统正常工作，让待测系统认为`Fake Object`就是“真的”依赖组件，实现细节可以非常简单，不需要具有真实依赖组件的很多特性，也不需要像`Test Stub`那样接受测试的需求，返回特定response给待测系统。

总之，`Fake Object`的实现比`Test Stub`和`Mock Object`简单，所以更加可以快速满足测试需求。不过如果我们需要控制依赖组件对待测系统的输入或输出，我们应该使用`Test Stub`和`Mock Object`。

#### Test Spy - monitor real ones; you can change my behavior
`Test Spy`是指一个待测系统依赖组件的替身，并且会捕捉和保存待测对象对依赖系统的输出，这个输出会用于测试代码中的验证。`Test Spy`主要用于记录和验证待测对象对依赖系统的输出。







#### Dummy Object - pass around but never actually used

`Dummy Object`对象是指为了调用被测试方法而传入的假参数，为什么说是假参数呢？实际上这些传入的`Dummy`对象并不会对测试有任何作用，仅仅是为了成功调用被测试方法。

比如有一个类的实例创建要求传入多个参数，这些参数里面没有可选参数，其中有几个参数不会对测试产生任何作用，这时我们就可以创建`Dummy`对象作为假参数去创建这个实例。

### 模块推荐
每个不同语言每个定义的推荐工具
Java
mockito
mockwire
moco
easymock
powermock
jmockit
Python
JavaScript

### 总结
这篇文章总结到我对`Test Double`相关概念的理解。表总结？


### 参考
* [MocksFakesStubsandDummies](http://xunitpatterns.com/Mocks,%20Fakes,%20Stubs%20and%20Dummies.html)
* [TestDouble](https://martinfowler.com/bliki/TestDouble.html)
* [doubles](http://python-doublex.readthedocs.io/en/latest/doubles.html)

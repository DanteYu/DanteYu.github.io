---
layout:     post
title:      我理解的自动化测试
subtitle:   ""
date:       2016-11-04 12:00:00
author:     DanteYu
tags:
    - automation
---

### 测试开发有着下面两种含义
1. 自动执行的测试用例
2. 辅助测试活动的工具

### 一个的测试框架应该做的事情：

#### 组成元素
编程语言 + 自然语言层 + 测试用例库 + 测试程序库 + 测试执行多样性 + 断言 + 测试报告

下面是RF的一个官方图，很好的表达了不同测试框架和测试库的关系以及测试框架和被测系统的关系
![rf](https://github.com/DanteYu/DanteYu.github.io/blob/master/_posts/images/rf.png?raw=true)

#### 需要提供的功能
##### 在测试用例管理提供的功能
- Test Fixture 执行测试的准备工作和清理工作 setup teardown
- Test Case最小的测试单元 一个测试点
- Test Suite 多个Test Case的集合
- tag标签
- 不同测试用例风格。关键字驱动、 数据驱动、 行为驱动Given When Then BDD、  ATDD

##### 在测试执行提供的功能 Test Runner
- 能够跑任意指定的test case。 Test Loader自动化加载需要跑的case到一个Test Suite中
- 跳过测试skip
- 期待失败。得到期待的测试失败。一个测试fail了，但是不会被当作为一个failure而记录在test report
- 数据驱动测试
- 内建标准库  timeout
- 逻辑编写

##### 在测试输出提供的功能
- 输出一个特定的报告
- 能够在命令行中运行
- 能够集成到CI，输出相应的报告

### 一个测试程序可以有下面这个公式
##### TestProgram = TestCase-->TestLoader-->TestSuite-->TestRunner-->TestResult

- Test Case： 框架的最小测试单元 一般用来用于当一个base class
- Test Suite： 单独测试单元的组合集合 形成测试套件
- Test Loader： 从模块和类中创建测试套件
- Test Result： 测试用例的结果保存实例，通常有测试框架调用
- Text TestRunner： 进行测试用例执行的实例，其中Text的意思是以文本形式显示测试结果

### 利用自动化测试金字塔来指导测试开发
- 金字塔底层是自动化测试的基础，主要包含单元测试和组件测试
- 金字塔中层是面向业务的自动化测试。调用产品提供的编程接口
- 金字塔顶层是少量的基于图形界面的自动化测试
- 金字塔上方是手工测试

![ideal automated testing pyramid](https://watirmelon.files.wordpress.com/2012/01/idealautomatedtestingpyramid.png)
![agile testing pyramid](https://image.slidesharecdn.com/agiletestingframework-theartofautomatedtesting-140428114238-phpapp02/95/agile-testing-framework-the-art-of-automated-testing-18-638.jpg?cb=1442514508)

#### 敏捷测试四象限里面有归纳出哪些测试是可以自动化的
![Agile_Testing_Quadrants](http://istqbexamcertification.com/wp-content/uploads/2014/12/Agile_Testing_Quadrants.jpg)

#### 一个关于自动化测试的mindmap

![mindmap](https://github.com/DanteYu/DanteYu.github.io/blob/master/_posts/images/atmindmap.png?raw=true)


#### 常用的测试框架/工具

##### 单元测试
- Junit
- unittest
- [X]unit

##### 功能性测试
- cucumber[BDD]
- concordion[BDD]
- Robotframework [ATDD]
- behave [BDD]
- JBehave[BDD]

##### JS
- Mocha
- Karma + Jasmine
- Protractor (https://angular.github.io/protractor/) JavaScript
- chai
- expect
- frisbyjs
- jasmine
- supertest

##### Python
- python + web driver + unittest
- python + behave
- python + request

##### java
- java + rest assured
- java + concordion + webdriver

##### Mobile
- robotium
- uiautomator
- appium
- calabash
- espresso

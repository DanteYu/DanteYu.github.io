---
layout:     post
title:      我的自动化测试理解
subtitle:   ""
date:       2016-11-04 12:00:00
author:     DanteYu
tags:
    - automation
---

测试开发
自动执行的测试用例
辅助测试活动的工具

以下场景可以适用于计算机辅助测试
1. 用程序完成需要反复执行的测试准备工作
2. 用程序产生测试数据，并让产品使用它们          example  test case generator
3. 用程序模拟用户操作
4. 用程序监控产品行为

测试任务中存在许多运用程序来提高工作效率的机会。测试人员需要抓住这些机会，综合运用多种开发技术来构建不同类型的测试工具

对应不同的自动化测试类型，但是大体的开发策略都是一致的

一个的测试框架应该做的事情：

组成元素： 编程语言 + 自然语言层 + 测试用例库 + 测试程序库 + 测试执行多样性 + 断言 + 测试报告

提供的功能：

在测试用例管理提供的功能：
     Test Fixture 执行测试的准备工作和清理工作 setup teardown
     Test Case最小的测试单元 一个测试点
     Test Suite 多个Test Case的集合
     标签
     不同测试用例风格 关键字驱动 数据驱动 行为驱动Given When Then BDD  ATDD

在测试执行提供的功能 Test Runner：
     能够跑任意指定的test case。 Test Loader 自动化加载需要跑的case到一个Test Suite中
     跳过测试skip
     期待失败。得到期待的测试失败。一个测试fail了，但是不会被当作为一个failure而记录在test report
     数据驱动测试
     内建标准库  timeout
     逻辑编写

在测试输出提供的功能
     输出一个特定的报告
     能够在命令行中运行
     能够集成到CI，输出相应的报告


TestProgram = TestCase ——>>TestLoader----->> TestSuite -----TestRunner----->> TestResult

Test Case  框架的最小测试单元 一般用来用于当一个base class
Test Suite  单独测试单元的组合集合 形成测试套件
Test Loader  从模块和类中创建测试套件
class TestResult 测试用例的结果保存实例，通常有测试框架调用
class TextTestRunner 进行测试用例执行的实例，其中Text的意思是以文本形式显示测试结果

利用自动化测试金字塔来指导测试开发
金字塔底层是自动化测试的基础，主要包含单元测试和组件测试
金字塔中层是面向业务的自动化测试。调用产品提供的编程接口
金字塔顶层是少量的基于图形界面的自动化测试
金字塔上方是手工测试

![ideal automated testing pyramid](https://watirmelon.files.wordpress.com/2012/01/idealautomatedtestingpyramid.png)
![agile testing pyramid](http://riis.com/images/posts/Recording-UI-Testing-with-XCUI-Swift/gregory1_fig01.jpg)

面向调试的测试代码
测试代码应该尽可能简单
测试代码应该容易阅读
测试代码需要合理的抽象和封装
测试代码应该检查每一步操作的结果，发现问题立刻报告
测试代码应该拥抱日志
除了文本日志，测试代码还可以记录其他形式的日志


常用的测试框架

单元测试
Junit
unittest
[X]unit

功能性测试
cucumber
concordion
Robotframework [ATDD]
behave [BDD]
JBehave[BDD]

JS:
https://code.google.com/p/rest-assured/
Mocha
Karma + Jasmine
Protractor (https://angular.github.io/protractor/) JavaScript
chai
expect
http://frisbyjs.com/

mobile：
robotium
uiautomator
appium
calabash
espresso



python 
python + web driver + unittest
python + behave
python + request

java
java + rest assured
java + concordion + webdriver

js
js + jasmine + protractor
js + mocha + chai + supertest
---
layout:     post
title:      我的自动化测试项目demo
subtitle:   ""
date:       2017-11-23 12:00:00
author:     DanteYu
tags:
    - automation
---

**Test Automation**一直是我工作中重要的内容，不同的公司有着不同目标的产品和不同背景的项目，自动化测试的实现也是不尽相同。

- 第一家公司主要用的是java+TestNG+selenium webdriver进行web UI测试
- 第二家公司主要用的是python+RobotFramework+unittest进行在linux下的通信产品测试
- 现在公司用的可就多了，目前项目在用，有过经验的和有过接触的有:
	- __UI testing__
    - webdriver+cucumber+Junit
    - protractor+jasmine
    - webdriver+concordion+Junit
    - codeceptjs
	- __API testing__
    - Pact(contract testing)
    - RestAssured+Junit
	- 同时在项目中自己也写过
    - python+webdriver+unittest
    - python+requests

为了总结平时所学，下面的repo会有一些简单(hello world级别)的demo。这些demo都是学习和实践过程中的产物。也希望这些测试项目能做到在新项目中“拿来即用”，减少自动化测试启动时间。

目前已经完成
* [python + behave + requests](https://github.com/DanteYu/Test_Automation_Demo/tree/master/APITesting/python_behave_requests)
* [python + webdriver + unittest](https://github.com/DanteYu/Test_Automation_Demo/tree/master/UITesting/python_webdriver_unittest)
* [js + jasmine + protractor](https://github.com/DanteYu/Test_Automation_Demo/tree/master/UITesting/js_jasmine_protractor)
* [js + codeceptjs + webdriverio](https://github.com/DanteYu/Test_Automation_Demo/tree/master/UITesting/js_codeceptjs_webdriverio)
* [js + mocha + chakram](https://github.com/DanteYu/Test_Automation_Demo/tree/master/APITesting/js_mocha_chakram)

repo地址： [MyDemo](https://github.com/DanteYu/Test_Automation_Demo)

希望自己能有时间不断增加更多的demo。

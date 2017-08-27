---
layout:     post
title:      codeceptjs学习笔记
subtitle:   安装和基本知识
date:       2017-08-26 12:00:00
author:     DanteYu
tags:
    - codeceptjs
    - nodejs
---

### codeceptjs介绍

[codeceptjs](https://github.com/Codeception/CodeceptJS)是[Codeception](http://codeception.com/)测试框架下的一个项目，是运行在nodejs的UI测试框架, 能使场景驱动的验收测试简单化。

"Modern Era Acceptance Testing Framework for NodeJS"是codeceptjs[官网](http://codecept.io/)最显著的标语，那作为一个UI测试框架，究竟有哪些特点：
* **从用户角度编写**。codeceptjs提供的API具有[declarative testing](https://support.saucelabs.com/hc/en-us/articles/115009283587-Imperative-v-Declarative-Testing)的特点，从用户行为的角度抽象出了许多与浏览器的交互动作，非常易读易懂
* **所有的交互动作来自于对象`I`**。易读易写易维护，对非技术人员也很友好
* **对多个后端API兼容**。codeceptjs支持多个webdriver实现 i.e. webdriverio, protractor and phantomjs, 我们可以很简单在他们之间切换。这些不同的webdriver实现在codeceptjs里面叫做*Helper*，正是下面这些Helper给对象`I`提供了actions
  * [Webdriverio](http://webdriver.io/)
  * [Protractor](http://www.protractortest.org/)
  * [Phantomjs](http://phantomjs.org/)
  * [Nigntmare](http://www.nightmarejs.org/)
  * [Appium](http://appium.io/)
  * [Selenium Webdriver](https://www.npmjs.com/package/selenium-webdriver)
* **同步**。我们不用关心js中的异步问题 i.e。 promises & callback。测试场景是线性的
* **基于[Mocha](http://mochajs.org/)测试框架**。
* **可以编写出易读易懂的场景驱动的验收测试**。可以写成BDD style
* **Smart locator**。Smart locators: use names, labels, matching text, CSS or XPath to locate elements
* **互动式debug shell**。可以在测试执行时随时暂停然后尝试不同的命令
* **提供了方便的命令行工具帮助创建测试**。可以通过命令一步创建tests, pageobjects, stepobjects

说了这么多特点，它长什么样子呢？一个简单的demo如下：
```javascript
Feature('CodeceptJS demo');

Scenario('check Welcome page on site', (I) => {
  I.amOnPage('/');
  I.see('Welcome');
});
```

是不是可以基本当plain text来读？:smile:

### codeceptjs安装

##### 环境检查
在安装codeceptjs之前，你要确保你的node版本是大于6.11的
```
$ node -v              
v8.3.0
```
##### 安装codeceptjs
使用npm全局安装codeceptjs，这样的话我们就可以在shell里面直接使用`codeceptjs`命令

`[sudo] npm install -g codeceptjs
`

本地安装, 这样的话我们可以执行`./node_modules/.bin/codeceptjs`

`npm install --save-dev codeceptjs
`

##### 安装browser driver
全局安装webdriverio
`[sudo] npm install -g webdriverio`

本地安装webdriverio
`npm install webdriverio --save-dev`

全局安装protractor
`[sudo] npm install -g protractor`

本地安装protractor
`npm install protractor --save-dev`

全局安装nightmare
`[sudo] npm install -g nightmare nightmare-upload`

本地安装nightmare
`npm install nightmare nightmare-upload --save-dev`

安装完之后你可以通过`codeceptjs`命令查看codeceptjs的版本和常用命令参数介绍
```
➜  codeceptjs-demo git:(master) ✗ codeceptjs
CodeceptJS v1.0.1

  Usage:  [options] [command]


  Options:

    -h, --help  output usage information


  Commands:

    init [path]                          Creates dummy config in current dir or [path]
    shell|sh [options] [path]            Interactive shell
    list|l [path]                        List all actions for I.
    def [path]                           List all actions for I.
    generate:test|gt [path]              Generates an empty test
    generate:pageobject|gpo [path]       Generates an empty page object
    generate:object|go [options] [path]  Generates an empty support object (page/step/fragment)
    generate:helper|gh [path]            Generates a new helper
    run [options] [test]                 Executes tests
    run-multiple [options] [suites...]   Executes tests multiple

```
其中参数`list`可以列出对象`I`所有的动作，也就是我们可以利用的浏览器交互API
```
➜  codeceptjs-demo git:(master) ✗ codeceptjs list
List of test actions: --
 WebDriverIO I.defineTimeout(timeouts)
 WebDriverIO I.amOnPage(amOnPage)
 WebDriverIO I.click(click)
 WebDriverIO I.doubleClick(doubleClick)
 WebDriverIO I.rightClick(rightClick)
 ..........
```


##### 安装selenium-standalone
因为这些helper i.e. `webdriverio`, `protractor`都是需要selenium作为后端与browser通信的服务，所以我们也需要安装selenium。

下面的命令会安装selenium standalone server以及相应的browser driver
```
npm install selenium-standalone@latest -g
selenium-standalone install
selenium-standalone start
```
详细的selenium-standalone资料请参考[这里](https://www.npmjs.com/package/selenium-standalone)

### codeceptjs基础知识

#### 命令行使用

###### `codeceptjs init`可以用在当我们需要初始化一个测试项目的时候。这个命令会创建和配置一些文件来搭建我们的测试环境。

```
➜  codeceptjs-init： codeceptjs init

  Welcome to CodeceptJS initialization tool
  It will prepare and configure a test environment for you

Installing to /Users/diyu/workspace/codeceptjs_demo/codeceptjs-init
? Where are your tests located? ./*_test.js
? What helpers do you want to use? WebDriverIO
? Where should logs, screenshots, and reports to be stored? ./output
? Would you like to extend I object with custom steps? Yes
? Do you want to choose localization for tests? English (no localization)
? Where would you like to place custom steps? ./steps_file.js
Configure helpers...
? [WebDriverIO] Base url of site to be tested http://localhost
? [WebDriverIO] Browser in which testing will be performed chrome
Steps file created at /Users/diyu/workspace/codeceptjs_demo/codeceptjs-init/steps_file.js
Config created at /Users/diyu/workspace/codeceptjs_demo/codeceptjs-init/codecept.json
Directory for temporary output files created at `_output`
Almost done! Create your first test by executing `codeceptjs gt` (generate test) command
➜  codeceptjs-init： ll
total 16
-rw-r--r--  1 diyu  staff   291B  8 27 22:39 codecept.json
drwxr-xr-x  2 diyu  staff    68B  8 27 22:39 output
-rw-r--r--  1 diyu  staff   281B  8 27 22:39 steps_file.js

```
如上所示，一些测试文件、配置文件和输出目录都创建好了。具体到这些文件是干嘛的，里面内容是什么，之后的学习笔记后提到。

###### `codeceptjs gt`会创建测试文件

```
➜  codeceptjs-init： codeceptjs gt
Creating a new test...
----------------------
? Filename of a test first
? Feature which is being tested first demo
Test for first was created in /Users/diyu/workspace/codeceptjs_demo/codeceptjs-init/first_test.js
➜  codeceptjs-init： ll
total 24
-rw-r--r--  1 diyu  staff   291B  8 27 22:39 codecept.json
-rw-r--r--  1 diyu  staff    66B  8 27 23:50 first_test.js
drwxr-xr-x  2 diyu  staff    68B  8 27 22:39 output
-rw-r--r--  1 diyu  staff   281B  8 27 22:39 steps_file.js
➜  codeceptjs-init： cat first_test.js

Feature('first demo');

Scenario('test something', (I) => {

});
```
first_test.js 就是被创建的测试文件

###### `codeceptjs run` 会运行测试。加上参数`--steps`会输出详细的过程

```
➜  codeceptjs-init: codeceptjs run --steps
CodeceptJS v1.0.1
Using test root "/Users/diyu/workspace/codeceptjs_demo/codeceptjs-init"

first demo --
 test something
 ✓ OK in 1ms

  OK  | 1 passed   // 2s
```

###### `codeceptjs gpo` 

#### 常用API解读

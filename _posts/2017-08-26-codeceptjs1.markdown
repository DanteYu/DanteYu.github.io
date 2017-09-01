---
layout:     post
title:      codeceptjs学习笔记
subtitle:   安装和基本知识 (In Progress)
date:       2017-08-26 12:00:00
author:     DanteYu
tags:
    - codeceptjs
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

是不是可以基本当plain text来读？:)

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

###### `codeceptjs run` 会运行测试。加上参数`--steps`会输出详细的执行过程

```
➜  codeceptjs-init: codeceptjs run --steps
CodeceptJS v1.0.1
Using test root "/Users/diyu/workspace/codeceptjs_demo/codeceptjs-init"

first demo --
 test something
 ✓ OK in 1ms

  OK  | 1 passed   // 2s
```

###### `codeceptjs gpo`会创建一个page object file。这个命令可以快速简单的帮助你实现page object设计模式

```
➜  codeceptjs-init codeceptjs gpo
Creating a new page object
--------------------------
? Name of a page object landingPage
? Where should it be stored ./pages/landingPage.js
Updating configuration file...
Page object for landingPage was created in /Users/diyu/workspace/codeceptjs_demo/codeceptjs-init/pages/landingPage.js
Use landingPagePage as parameter in test scenarios to access it
➜  codeceptjs-init ll
total 24
-rw-r--r--  1 diyu  staff   375B  8 29 21:30 codecept.json
-rw-r--r--  1 diyu  staff    66B  8 27 23:50 first_test.js
drwxr-xr-x  2 diyu  staff    68B  8 27 22:39 output
drwxr-xr-x  3 diyu  staff   102B  8 29 21:30 pages
-rw-r--r--  1 diyu  staff   281B  8 27 22:39 steps_file.js
```
landingPage.js被创建在pages目录下面，这个文件里面会有landingPage的元素定位和基本操作。一个简单的demo如下

```javascript
'use strict';

let I;

module.exports = {

  _init() {
    I = actor();
  },

  fields: {
    email: '#user_basic_email',
    password: '#user_basic_password'
  },
  submitButton: {css: '#new_user_basic input[type=submit]'},

  sendForm(email, password) {
    I.fillField(this.fields.email, email);
    I.fillField(this.fields.password, password);
    I.click(this.submitButton);
  }
}
```
通过参数传递的方式，我们可以在测试文件里面注入页面元素和操作
```javascript
Feature('CodeceptJS Demonstration');

Before((I) => { // or Background
  I.amOnPage('http://simple-form-bootstrap.plataformatec.com.br/documentation');
});

Scenario('test some forms', (I, docsPage) => {
  docsPage.sendForm('hello@world.com','123456');
  I.see('User is valid');
  I.dontSeeInCurrentUrl('/documentation');
});
```
这样的话，最简单的page object就可以用了

#### Actions and Assertions

下面通过一个简单的demo来看下codeceptjs测试文件长什么样子

```javascript
Feature('CodeceptJS Demonstration');

Scenario('test some forms', (I) => {
  I.amOnPage('http://simple-form-bootstrap.plataformatec.com.br/documentation');
  I.fillField('Email', 'hello@world.com');
  I.fillField('Password', '123456');
  I.checkOption('Active');
  I.checkOption('Male');
  I.click('Create User');
  I.see('User is valid');
  I.dontSeeInCurrentUrl('/documentation');
});
```

可以看出
* 所有的页面操作都由对象`I`调用执行，而`I`的方法都来自于Helper
* see()，dontSee()方法是用来做验证的。可以在页面上找到指定元素进行验证.如果第二个参数提供，可以缩小元素查找范围。`I.see('User is valid', '.alert-success');`
* Feature和Scenario来源于mocha
* fillField(), checkOption()和click()这些方法工作原理类似，都是通过name, css和xpath找到元素并与之交互

#### Grabber
当我们需要从页面中抓取元素然后用到后续测试的时候，我们可以使用带`grab`前缀的方法。它的用法如下
```javascript
var assert = require('assert');

Feature('CodeceptJS Demonstration');

Scenario('test page title', function*(I) {
  I.amOnPage('http://simple-form-bootstrap.plataformatec.com.br/documentation');
  var title = yield I.grabTitle();
  assert.equal(title, 'Example application with SimpleForm and Twitter Bootstrap');
});
```
grad()要被使用在generator里面，所以带有yield关键字

#### Before/After
codeceptjs也提供了测试框架必不可少的用作测试准备和清理的方法。

下面的测试代码片段中Before()会在所有Scenario()方法之前执行。Before()也可以换成Background()。

```javascript
Feature('CodeceptJS Demonstration');

Before((I) => { // or Background
  I.amOnPage('http://simple-form-bootstrap.plataformatec.com.br/documentation');
});

Scenario('test some forms', (I) => {
  I.click('Create User');
  I.see('User is valid');
  I.dontSeeInCurrentUrl('/documentation');
});

Scenario('test title', (I) => {
  I.seeInTitle('Example application');
});
```

#### debug
codeceptjs可以通过两种方式进行测试代码开发中的调试
* 在测试文件中使用`pause()`方法

```
➜  codeceptjs-init codeceptjs run --steps
CodeceptJS v1.0.1
Using test root "/Users/diyu/workspace/codeceptjs_demo/codeceptjs-init"

CodeceptJS Demonstration --
 test some forms
 • I am on page "http://simple-form-bootstrap.plataformatec.com.br/documentation"
 • I fill field "Email", "hello@world.com"
 • I fill field "Password", "123456"
 • I check option "Active"
 Interative debug session started
 Use JavaScript syntax to try steps in action
 Press ENTER to continue
 I.
Exiting interactive shell....
 • I check option "Male"
 • I click "Create User"
 • I see "User is valid"
 • I dont see in current url "/documentation"
 ✓ OK in 12581ms
```
注意到中间突然出现了交互式的debug session，这就是pause()的作用

* 下面这种`codeceptjs shell`来进行调试

```
➜  codeceptjs-init codeceptjs shell
String interactive shell for current suite...
 Interative debug session started
 Use JavaScript syntax to try steps in action
 Press ENTER to continue
 I.amOnPage('www.hupu.com')
 I.
```
通过这个互动的debug session，我们可以一步步的输入不同的action来观察浏览器的行为

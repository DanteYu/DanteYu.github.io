---
layout:     post
title:      codeceptjs学习笔记
subtitle:   Page Object设计模式
date:       2017-09-09 12:00:00
author:     DanteYu
tags:
    - codeceptjs
---

这篇文章会介绍UI测试最常用的设计模式在codeceptjs的应用 - **Page Object**

#### Benefit

Page Object有哪些好处？
  *  代码重用。可以在多个测试场景里面重用代码，减少重复的代码
  *  易维护。如果系统有变动，那么只需要在一个地方进行修改

#### codeceptjs的Page Object

`codeceptjs gpo`可以帮助你快速创建模板和进行配置

```
➜  codeceptjs-demo git:(master) ✗ codeceptjs gpo
Creating a new page object
--------------------------
? Name of a page object LandingPage
? Where should it be stored ./pages/LandingPage.js
Updating configuration file...
Page object for LandingPage was created in /Users/diyu/workspace/codeceptjs_demo/codeceptjs-demo/pages/LandingPage.js
Use landingPagePage as parameter in test scenarios to access it
```
模板文件会被创建在项目根目录下的pages目录中，内容如下

```js
'use strict';
let I;

module.exports = {

  _init() {
    I = actor();
  }

  // insert your locators and methods here
}
```

把这个文件补充完整，就会变为下面这样

```js
'use strict';
let I;

module.exports = {

  _init() {
    I = actor();
  },

  // setting locators
  fields: {
    email: '#user_basic_email',
    password: '#user_basic_password'
  },
  submitButton: {css: '#new_user_basic input[type=submit]'},

  // introducing methods
  sendForm(email, password) {
    I.fillField(this.fields.email, email);
    I.fillField(this.fields.password, password);
    I.click(this.submitButton);
  }
}
```

上面的page object文件写好之后，我们应该把它加入到测试文件中
```js
Scenario('login', (I, loginPage) => {
  loginPage.sendForm('john@doe.com','123456');
  I.see('Hello, John');
});
```

还需要加入到在配置文件中
```json
"include": {
  "I": "./steps_file.js",
  "landingPage": "./pages/landingPage.js"
}
```

#### generator在page object中的应用

我们可以在po文件中通过generator来使用grab*()方法

```js
'use strict';
let I;

module.exports = {

  _init() {
    I = actor();
  },

  // setting locators
  container: "//div[@class = 'numbers']",
  mainItem: {
    number: ".//div[contains(@class, 'numbers__main-number')]",
    title: ".//div[contains(@class, 'numbers__main-title-block')]"
  },

  // introducing methods
  openMainArticle: function* () {
    I.waitForVisible(this.container)
    let _this = this
    let title;
    yield within(this.container, function*(){
      title = yield I.grabTextFrom(_this.mainItem.number);
      let subtitle = yield I.grabTextFrom(_this.mainItem.title);
      title = title + " " + subtitle.charAt(0).toLowerCase() + subtitle.slice(1);
      yield I.click(_this.mainItem.title)
    })
    return title;
  }
}
```

```js
Scenario('login2', (I, mainPage, basePage) => {
  let title = yield* mainPage.openMainArticle()
  basePage.pageShouldBeOpened(title)
});
```

#### PageFragment

PageFragment是指在页面比较独立的部分，比如页面中一些独立组件(i.e. 弹出框)和小工具。我们可以把这可能每个页面都在用的公共部分抽取出来，单独的处理。

使用`codeceptjs go --type fragment`能够创建模板文件

```
➜  codeceptjs-init codeceptjs go --type fragment
Creating a new fragment object
--------------------------
? Name of a fragment object LandingPage
? Where should it be stored ./fragments/LandingPage.js
Updating configuration file...
Fragment object for LandingPage was created in /Users/diyu/workspace/codeceptjs_demo/codeceptjs-init/fragments/LandingPage.js
Use landingPageFragment as parameter in test scenarios to access it
```
配置文件如下
```
"include": {
  "I": "./steps_file.js",
  "landingPagePage": "./pages/landingPage.js",
  "landingPageStep": "./steps/LandingPage.js",
  "landingPageFragment": "./fragments/LandingPage.js"
},
```

对于page fragment推荐的使用方式是包含一个组件的root locator，然后在page fragment中的方法使用within()去缩小范围。

```js
let I;
// fragments/modal.js
module.exports = {

  _init() {
    I = actor();
  },

  root: '#modal',

  // we are clicking "Accept: inside a popup window"
  accept() {
    within(this.root, function() {
      I.click('Accept');
    });
  }
}
```

#### StepObjects

StepObjects代表涉及多个页面的复杂操作流程，实现了业务价值。

使用`codeceptjs go --type step`能够创建模板文件。

```
➜  codeceptjs-init codeceptjs go --type step
Creating a new step object
--------------------------
? Name of a step object LandingPage
? Where should it be stored ./steps/LandingPage.js
Updating configuration file...
Step object for LandingPage was created in /Users/diyu/workspace/codeceptjs_demo/codeceptjs-init/steps/LandingPage.js
Use landingPageStep as parameter in test scenarios to access it
```

配置文件如下：
```js
"include": {
  "I": "./steps_file.js",
  "landingPagePage": "./pages/landingPage.js",
  "landingPageStep": "./steps/LandingPage.js"
}
```

```js
let I, userPage, permissionPage;
module.exports = {

  _init() {
    I = actor();
    userPage = require('../pages/user');
    userPage._init();
    permissionPage = require('../pages/permissions');
    permissionPage._init();
  },

  createUser(name) {
    // action composed from actions of page objects
    userPage.open();
    userPage.create(name);
    permissionPage.activate(name);
  }
};
```

#### Actor

在我们刚开始初始化的时候  `codeceptjs init`，有一个步骤会问你需要custom step files不。这个文件的目的是可以让你通过`steps_file.js`来扩展`I`对象的方法。

我们在`steps_file.js`中加入新的一个方法foo()

```js
'use strict';
// in this file you can append custom step methods to 'I' object

module.exports = function() {
  return actor({

    // Define custom steps here, use 'this' to access default methods of I.
    // It is recommended to place a general 'login' function here.

    foo : function(){
    	console.log('I am customized step foo for object I');
    }

  });
}
```

然后可以在测试文件中直接作为`I`的方法来进行调用了。

```js
Feature('within demo');

Scenario('login github', {retries: 2}, (I, landingPage)=>{
	I.amOnPage('http://www.baidu.com');
	I.foo();
	landingPage.land();
	// I.see('test');
	I.say('Above is the link for baidu search engine');
});
```

要想在`steps_file.js`中使用`I`对象，需要使用`self`代替

```js
'use strict';
// in this file you can append custom step methods to 'I' object

module.exports = function() {
  return actor({

    login: function(email, password) {
      this.fillField('Email', email);
      this.fillField('Password', password);
      this.click('Submit');
    }
  });
}
```

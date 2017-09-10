---
layout:     post
title:      codeceptjs学习笔记
subtitle:   Actions用法
date:       2017-09-05 12:00:00
author:     DanteYu
tags:
    - codeceptjs
---

这篇文章会介绍一些与页面交互最常用的actions及其用法。

#### 定位元素

和其他框架定位元素一样，CSS/Xpath/ID/Name/Text等都是常用的方法。对于webdriverio这个helper来说，基本每一个方法都会接受css和xpath。
```javascript
I.seeElement('.user'); // element with CSS class user
I.seeElement('//button[contains(., "press me")]'); // button
```
默认情况下，因为codeceptjs会去猜locator的类型，所以你不用特别指明。我们也可以通过strict locator来告诉codeceptjs locator的类型。

**strict locator** 就是一组键值对，key表示的是locator的类型，value表示的是locator的值。

```javascript
// locate from element by css
I.seeElement({css: 'div.user'});
// locate form element by xpath
I.seeElement({xpath: '//div[@class=user]'});
// locate form element by name
I.seeElement({name: 'password'});
// locate element by text
I.seeElement({text: 'press me'});
// locate element by id
I.seeElement({id: 'users'});
```

#### 点击元素

点击link或是button算是每个页面都会有的基本操作了。默认情况下，codeceptjs会采用“完全匹配”的方式通过button text来查找元素并且点击元素。
```javascript
// search for link or button
I.click('Login');
```
如果没有找到“完全匹配”的，会采用“部分匹配”的方式进行查找。对于可以点击的图片，会查找图片对应的`alt`属性。i.e. `<img src="/i/eg_tulip.jpg"  alt="imagetext" />` “imagetext”就是我们可以查找的文本。

为了缩小的查找的范围，click()方法的第二个参数可以接受一个locator来限定查找的区域。

```javascript
I.click('Login', '.nav'); // search only in .nav
I.click('Login', {css: 'footer'}); // search only in footer
```

strict locator和css id xpath都是可以用于click()中来缩小范围。不过这样的话，元素定位的范围就不仅限于link和button了。任何被找到的元素都会被点击。
```javascript
// click element by CSS
I.click('#signup');
// click element located by name inside a form
I.click({name: 'submit'}, '#user>form');
```

#### 填充文本框

填充文本框使用fillField()方法，支持name,lable, Xpath和CSS等locator类型。

```javascript
// we are using label to match user_name field
I.fillField('Name', 'Miles');
// we can use input name
I.fillField('user[email]','miles@davis.com');
// we are using CSS
I.fillField('#user_name', 'Miles');
I.fillField('#user_email','miles@davis.com');
```
#### 选取下拉菜单
下拉菜单选值使用selectOption()方法，支持name,lable, Xpath和CSS等locator类型。
```javascript
// select element by label, choose option by text
I.selectOption('Gender','Male');
// select element by label, option by value
I.selectOption('#user_gender','m');
```

#### 验证
为了验证我们是否到了期望的页面或是完成了期望的行为，我们需要方法来判断页面上是否有我们需要的元素显示出来。`I.see*()`和`I.dontSee*()`方法可以验证当前用户看到的页面上元素是否存在。

最常用的是`I.see()`和`I.dontSee()`.下面的列子可以验证页面上的文本是否存在。和其他方法类似，第二个参数可以缩小查找范围。

```javascript
// Just a visible text on a page
I.see('Hello');
// text inside .msg element
I.see('Hello', '.msg');
// opposite
I.dontSee('Bye');
```

`I.seeElement()`和`I.dontSeeElement()`可以判断元素的出现与否

```javascript
I.seeElement('.notice');
I.dontSeeElement('.error');
```

除此之外，页面的URL,title和文本框里面的值都是可以验证的。更多的方法请参考[这里](http://codecept.io/helpers/WebDriverIO/)。
```javascript
I.seeInCurrentUrl('/user/miles');
I.seeInField('user[name]', 'Miles');
I.seeInTitle('My Website');
```

#### 抓取元素
某些时候你需要从页面获取一些数据来做验证或是下一个测试步骤的输入，比如页面显示出来的文本。所有已`grab*`开头的方法可以满足这个需求。

```javascript
I.fillField('email', 'miles@davis.com')
I.click('Generate Password');
let password = yield I.grabTextFrom('#password');
I.click('Login');
I.fillField('email', 'miles@davis.com');
I.fillField('password', password);
I.click('Log in!');
```

上面的`grabTextFrom()`方法用于取出元素的文本。所有的`grab*()`方法都要用于生成器里面。
```javascript
Scenario('use page title', function*(I) {
  // ...
  var password = yield I.grabTextFrom('#password');
  I.fillField('password', password);
});
```

#### 等待wait
在日常造成UI测试失败的原因中，render太慢造成元素在页面显示不出来然后操作无法完成十分常见。于是在各大的测试框架中`wait*()`相关方法的出现就用来解决这个问题。

```javascript
I.waitForElement('#agree_button', 30); // secs
// clicks a button only when it is visible
I.click('#agree_button');
```

#### SmartWait

Selenium Webdriver提供了两种不同类型的waits:
  * **implicit wait** -> 让webdriver在当定位元素的时候，如果元素没有立即出现，需要等待一段时间。这个作用域是DOM全局的。
  * **explicit wait** -> 让webdriver在继续执行操作之前，等待某个特定的情况发生。比如在点击某一个link之前，需要等待link加载完成

codeceptjs的SmartWait就是implicit wait的实现。如果需要定位的元素还没有出现，codeceptjs会在失败之前等待一段时间。

SmartWait是配置在Helper里面的，比如下面的`smartWait: 5000`

```javascript
"helpers": {
  "WebDriverIO": {
    "url": "http://simple-form-bootstrap.plataformatec.com.br",
    "browser": "chrome",
    "smartWait": 5000
  }
},
```

注意到SmartWait只在strict locator和标识了type的locator的方法中起作用
```javascript
I.click('Login'); // DISABLED, not a locator
I.fillField('user', 'davert'); // DISABLED, not a specific locator
I.fillField({name: 'password'}, '123456'); // ENABLED, strict locator
I.click('#login'); // ENABLED, locator is CSS ID
I.see('Hello, Davert'); // DISABLED, Not a locator
I.seeElement('#userbar'); // ENABLED
I.dontSeeElement('#login'); // DISABLED, can't wait for element to hide
I.seeNumberOfElements('button.link', 5); // DISABLED, can wait only for one element
```

#### IFrames
`within()`方法可以用于IFrame的定位，在`within()`内部的方法就会在IFrame中执行。

```javascript
within({frame: "#editor"}, () => {
  I.see('Page');
});
```

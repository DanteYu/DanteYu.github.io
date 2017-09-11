---
layout:     post
title:      codeceptjs学习笔记
subtitle:   高级用法
date:       2017-09-10 12:00:00
author:     DanteYu
tags:
    - codeceptjs
---

这篇文章会介绍codeceptjs的一些高级用法

#### 数据驱动测试

基本每个测试框架都会提供用数据驱动跑测试用的方法，codeceptjs也不例外。

`Data().Scenario()`可以接受数据，然后让不同的数据运行在相同的场景中。

`new DataTable()`常用于建立数据表来做数据驱动的测试。我们可以把数据单独定义在测试文件中，然后把数据对象做为Data()的参数传入，注意在Scenario()中第二个参数方法，参数`current`用于获得数据的值。
```js
// define data table inside a test or load from another module
let accounts = new DataTable(['login', 'password']); //
accounts.add(['davert', '123456']); // adding records to a table
accounts.add(['admin', '123456']);

// Pass dataTable to Data()
// Use special param `current` to get current data set
Data(accounts).Scenario('Test Login', (I, current) => {
  I.fillField('Username', current.login); // current is reserved!
  I.fillField('Password', current.password);
  I.click('Sign In');
  I.see('Welcome '+ current.login);
});
```

我们也可以在单独的测试文件中定义，然后在测试文件中引入。

```js
'use strict';

var dataset1 = function(){

	var dataset1 = {};

	var accounts = new DataTable(['login', 'password']); //
	accounts.add(['davert', '123456']); // adding records to a table
	accounts.add(['admin', '123456']);

	dataset1.accounts = accounts;

	return dataset1;
}

module.exports = dataset1;
```

然后在另外一个测试文件中引入即可
```js
var dataset1 = require('../testdata/dataset1.js')();

Feature('within demo');

Data(dataset1.accounts).Scenario('Test Login', (I, current) => {
  console.log(current.login); // current is reserved!
  console.log(current.password);
});
```

除了`new DataTable()`, 生成器也可以定义数据集

```js
Data(function*() {
  yield { user: 'davert', pwd: 'test'};
  yield { user: 'andrey', pwd: 'test'};
}).Scenario('test generator', (I, current) => {

console.log(current.user);
console.log(current.pwd);
})
```
测试结果会是

```
test generator | [object Object]
davert
test
✓ OK in 0ms

test generator | [object Object]
andrey
test
✓ OK in 0ms
```

数组用作数据集

```js
Data(['1', '2', '3']).Scenario('test array', (I, current)=>{
	console.log(current);
})
```

测试结果会是
```
test array | 1
1
✓ OK in 1ms

test array | 2
2
✓ OK in 0ms

test array | 3
3
✓ OK in 0ms
```

方法返回值用作数据集

```js
Data(function(){
	return ['1', '2'];
}).Scenario('test array', (I, current)=>{
	console.log(current);
})
```
结果会是
```
test array | 1
1
✓ OK in 0ms

test array | 2
2
✓ OK in 0ms
```

#### Groups

对测试场景进行加tag分组可以帮我们灵活的执行指定的测试场景和场景组合。Tag功能也算是测试框架必有的功能之一。

`Scenario('description @tag')` 如这个命令所见，我们只需要在`Scenario()`方法第一参数test name里面加上`@tag`即可。当运行`codeceptjs run --grep @tag`，所有标记了`@tag`的测试场景都会执行

```js
Scenario('update user profile @slow')
codeceptjs run --grep @slow
```

正则表达式提供了更多的灵活性

```
Use regex for more flexible filtering:

--grep (?=.*@smoke2)(?=.*@smoke3) - run tests with @smoke2 and @smoke3 in name
--grep @smoke2|@smoke3 - run tests with @smoke2 or @smoke3 in name
--grep ((?=.*@smoke2)(?=.*@smoke3))|@smoke4 - run tests with (@smoke2 and @smoke3) or @smoke4 in name
--grep (?=.*@smoke2)^(?!.*@smoke3) - run tests with @smoke2 but without @smoke3 in name
```

#### Debug

codeceptjs提供了debug模式来提供更多的信息。使用`--debug`选项 `codeceptjs run --debug`。

使用`pause()`方法能够暂停执行进入交互模式。

#### 并发多个场景执行

codeceptjs能够并行的执行多个测试套件。这就意味着我们可以在不同浏览器或是不同配置项中同时运行相同的测试。

首先我们需要在配置文件中加入`multiple`

```js
"multiple": {
  "basic": {
    // run all tests in chrome and firefox
    "browsers": ["chrome", "firefox"]
  },

  "smoke": {
    // run only tests containing "@smoke" in name
    "grep": "@smoke",

    // use firefox and different chrome configurations
    "browsers": [
      "firefox",
      {"browser": "chrome", "windowSize": "maximize"},
      // replace any config values from WebDriverIO helper
      {"browser": "chrome", "windowSize": "1200x840"}
    ]
  }
}
```

在`multiple`下面我们定义了两个测试套件，`basic`测试套件由于没有定义`grep`，所以会在chrome和firefox中运行所有测试；`smoke`测试套件定义了`grep`为`@smoke`，所以会在firefox和chrome maximize和chrome 1200*840三个浏览器中运行带有`@smoke`标签的。

定义好之后，我们可以使用命令`codeceptjs run-multiple`来运行定义在`multiple`里面的测试套件和浏览器。

更多的`codeceptjs run-multiple`使用方法如下

* `codeceptjs run-multiple --all` -> 在所有的浏览器上跑所有的测试套件
* `codeceptjs run-multiple basic` -> 在所有的浏览器上跑`basic`测试套件
* `codeceptjs run-multiple basic:chrome` -> 在chrome上跑`basic`测试套件
* `codeceptjs run-multiple basic:chrome smoke:firefox` -> 在chrome上跑`basic`测试套件和firefox上跑`smoke`测试套件
* `codeceptjs run-multiple basic --grep signin --reporter mocha-junit-reporter` -> 在所有的浏览器上跑`basic`测试套件中带有`signin` tag的测试场景，并且使用mocha-junit-reporter作为reporter
* `codeceptjs run-multiple regression -c path/to/config` -> 在所有浏览器上跑`regression`测试套件，使用指定的conf文件

执行完成之后会产生类似下面这样的产出，相应的文件夹会自动被创建。

```
➜  codeceptjs-init ll ./output
drwxr-xr-x  2 diyu  staff    68B  9 10 22:46 smoke_browser_chrome_1
drwxr-xr-x  2 diyu  staff    68B  9 10 22:47 smoke_browser_firefox_1
drwxr-xr-x  2 diyu  staff    68B  9 10 22:46 smoke_browser_firefox_2
```

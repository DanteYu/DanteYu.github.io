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

```javascript
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

#### Bootstrap & Teardown

Codeceptjs提供了机制来让定制代码在测试运行前和运行后分别执行，比如启动和停止webdriver服务。`Bootstrap` & `Teardown` 配置就可以帮我们实现这个机制。

下面几种方式都可以实现`Bootstrap` & `Teardown`功能
* JS file executed as is (synchronously).
* JS file exporting function with optional callback for async execution.
* JS file exporting an object with bootstrap and teardown methods.
* Inside JS config file

下面是一个实例

codeceptjs是依赖于selenium-standalone的，所以在执行codeceptjs之前，`selenium-standalone start`需要被执行。但是在实际应用场景中，比如持续集成中的自动化测试执行，我们不能保证每次执行测试的服务器都是安装好此模块和启动这个服务的；自动化安装和启动服务就变为必不可少。



首先，我们要先定义两个脚本分别代表start webdriver和stop webdriver

`selenium-standalone-start.js`

```js
const selenium = require('selenium-standalone');

module.exports = function(done) {
  selenium.install({
    baseURL: 'https://selenium-release.storage.googleapis.com',
    version: '3.5.3',
    drivers: {
      chrome: {
        version: '2.32',
        arch: process.arch,
        baseURL: 'https://chromedriver.storage.googleapis.com'
      },
      ie: {
        version: '3.5.1',
        arch: process.arch,
        baseURL: 'https://selenium-release.storage.googleapis.com'
      },
      firefox: {
        version: '0.18.0',
        arch: process.arch,
        baseURL: 'https://github.com/mozilla/geckodriver/releases/download'
      }
    }
}, function (err) {
    if (err) return done(err);
    selenium.start(function (err, child) {
      if (err) return done(err);
      selenium.child = child;
      done();
    });
  })
}
```

`selenium-standalone-stop.js`

```js
var selenium = require('selenium-standalone');

module.exports = function(done) {
  selenium.child.kill();
  done();
}
```

接着我们在codecept.json中配置`bootstrap`和`teardown`即可。配置完后即可在没有使用`selenium-standalone start`的情况下，自动开启selenium服务，运行测试和关闭selenium服务。

```
"bootstrap": "./util/selenium-standalone-start.js",
"teardown": "./util/selenium-standalone-stop.js",
```

#### Helpers

Helper是一个封装类，封装了多个libraries并且提供了统一的接口。Helper类给`I`对象提供了不同的方法。

当然，我们也可以自己定义Helper，定制化的Helper能够提供更多的方法。

Helper的创建可以通过命令 `codeceptjs gh`

```
➜  codeceptjs-init codeceptjs gh
Creating a new helper
--------------------------
? Name of a Helper MyHelper
? Where should it be stored ./custom_helpers.js
Updating configuration file...
Helper for MyHelper was created in /Users/diyu/workspace/codeceptjs_demo/codeceptjs-init/custom_helpers
```

这个命令会创建下列文件。这个文件中除了以underscore开始的方法，都会被加入到`I`对象中当做一个test actions。

```js
'use strict';

class MyHelper extends Helper {

  // before/after hooks
  _before() {
    // remove if not used
  }

  _after() {
    // remove if not used
  }

  // add custom methods here
  // If you need to access other helpers
  // use: this.helpers['helperName']

}
```

除了自定义的方法，定制Helper还可以通过`this.helpers['drivername']`来获取Helper的API以及通过`this.config`来获取Conf文件的信息。

Helpers里面的Hooks可以由下面方法实现。当定制的Helper被加到项目中后，定义的Hooks会作用于每一个测试文件。

* _init - before all tests。在所有测试文件执行前执行
* _before - before a test。在一个Sceanrio()之前执行
* _after - after a test。在一个Scenario()之后执行
* _beforeStep - before each step。在一个step之前执行
* _afterStep - after each step。在一个step之后执行
* _beforeSuite - before each suite。在一个Feature()之前执行
* _afterSuite - after each suite。在一个Feature()之后执行

当测试文件定义Before()和After()，这个两个方法会在_before()和_after()之中执行。
> _before() --> Before() --> After() --> _after()

```js
'use strict';

class MyHelper extends Helper {

  _init(){
    console.log('this is the _init method from MyHelper');
    return;
  };

  _before(){
    console.log('this is the _before method from MyHelper');
    return;
  };

  _after(){
    console.log('this is the _after method from MyHelper');
    return;
  };

  _beforeSuite(){
    console.log('this is the _beforeSuite method from MyHelper');
    return;
  };

  _afterSuite(){
    console.log('this is the _afterSuite method from MyHelper');
    return;
  };

  _beforeStep(){
    console.log('this is the _beforeStep method from MyHelper');
    return;
  };

  _afterStep(){
    console.log('this is the _afterStep method from MyHelper');
    return;
  };

sayHelloWorld(){
  console.log('Hello world in MyHelper');
  return;
};

openBaiduByWebdriver(){
  let client = this.helpers['WebDriverIO'].browser;
  client.url('https://www.baidu.com/');
  console.log(client.getUrl());
  return;
};

getConf(){
  let defaulthost = this.config.defaultHost;
  console.log(defaulthost);
  return;
}

}

module.exports = MyHelper;
```

以及在codeceptjs配置文件中加入下面选项

```
"helpers": {
  "WebDriverIO": {
    "url": "http://simple-form-bootstrap.plataformatec.com.br",
    "browser": "chrome"
  },
  "MyHelper": {
    "require": "./custom_helpers.js"
  }
}
```

#### Custom Hooks

我们可以通过Hooks来扩展codeceptjs的功能，codeceptjs提供了API来暴露了自己的内部机制，让我们可以创建自己的定制化功能。

当codeceptjs在本地安装成功后，我们可以通过`require(‘codeceptjs’)`去访问codeceptjs的下面这些内部对象.

* codecept: test runner class
* config: current codecept config
* event: event listener
* recorder: global promise chain
* output: internal printer
* container: dependency injection container for tests, includes current helpers and support objects
* helper: basic helper class
* actor: basic actor (I) class

为了让这些hooks生效，我们需要在配置文件中加入相应的hooks文件，可如下配置

```js
"hooks": [
  "./server.js",
  "./data_builder.js",
  "./report_notification.js"
]
```

下面是一些简单的列子

##### config: current codecept config

我们可以在测试文件中直接访问配置文件

```js
let config = require('codeceptjs').config.get();

Scenario.only('access config', {retries: 2}, (I)=>{

	console.log(config.timeout);

  if(config.tests = "./spec/s*_test.js"){
    console.log("yes, it is s*test.js")
  };
});
```

##### event: event listener

通过event listener我们可以监听事件和触发行为。

用`event.test.before`来举例，类似下面的文件会被创建

```js
const event = require('codeceptjs').event;

module.exports = function() {

  event.dispatcher.on(event.test.before, function (test) {

    console.log('--- I am before test --');

  });
}
```

然后我们在配置文件里面加上这个文件
```js
"hooks": ["./util/custom_hooks/event_listener.js"],
```

这样在执行测试的时候，这个event listener定义的方法就会被自动触发。

##### output: internal printer

output模块提供了四种级别的信息显示模式
* **default**：使用`output.print()`显示基本信息
* **steps**：使用`--steps`显示执行的step
* **debug**： 使用`--debug`显示执行的step，以及`output.debug()`带来的debug信息
* **verbose**：使用`--verbose`显示debug的信息，以及`output.log()`带来的log信息

```js
Scenario.only('access config', {retries: 2}, (I)=>{

	output.print(config.timeout);
	output.print('This is basic information');
	output.debug('This is debug information');
	output.log('This is verbose logging information');
  if(config.tests = "./spec/s*_test.js"){
    output.print("yes, it is s*test.js")
  };
});
```

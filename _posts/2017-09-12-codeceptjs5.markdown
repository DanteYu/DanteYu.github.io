---
layout:     post
title:      codeceptjs学习笔记
subtitle:   配置文件解释
date:       2017-09-12 12:00:00
author:     DanteYu
tags:
    - codeceptjs
---

这篇文章会介绍codeceptjs配置文件的基本用法。

`codeceptjs init`创建的文件`codecept.json`就是整个codeceptjs项目的配置文件。这个文件处于整个项目的根目录下。

其大概的内容如下：

```xml
{
  "output": "./output", //配置哪里存放失败的测试截图等

  "helpers": {   //配置helper
    "WebDriverIO": {   //配置webdriverio作为helpers
      "url": "http://simple-form-bootstrap.plataformatec.com.br", //配置base url
      "browser": "chrome",  //配置运行的浏览器
      "smartWait": 5000，   //optional，配置Smart Wait时间
      "restart": false,  //optional，配置是否在测试场景之间重启浏览器，默认值是true
      "keepBrowserState": false, //optional, 当restart设置为false的时候，配置是否在测试之间保持浏览器的状态， 默认值是false
      “disableScreenshots”: false, //optional, 配置是否在测试失败时存储截图，默认值是false
      "uniqueScreenshotNames": false, //optional, 如果有同名的测试场景在不同的测试套件里面，配置是否防止测试截图的覆盖，默认值是false
      "keepCookies": false, //optional, 当restart设置为false的时候，配置是否在测试之间保持cookies,默认值是false
      "windowSize": "maximize", //optional, 设置浏览器窗口大小。值可以为"maximize"或格式为"640*480"的尺寸
      "waitForTimeout": 5000, //optional, 给所有wait*()方法设定的默认等待时间. 默认值是1000
      "timeouts": {   // 配置超时时间。webdriverio的timeouts为键值对形式
      "script": 60000,
      "page load": 10000
      },
      "manualStart": false, //optional, 如果设置为true，则在测试之前不会启动浏览器，而需要在helper中加上方法`this.helpers["WebDriverIO"]._startBrowser()`来人工启动。默认值是false
      "url": "YOUR_DESIRED_HOST", //需要连接到cloud provider上去进行测试 比如browserstack或sauce lab
      "user": "YOUR_BROWSERSTACK_USER",
      "key": "YOUR_BROWSERSTACK_KEY",
      "desiredCapabilities": {
        "browserName": "chrome",
        // only set this if you're using BrowserStackLocal to test a local domain
        // "browserstack.local": true,
        // set this option to tell browserstack to provide addition debugging info
        // "browserstack.debug": true,
        "proxy": {   配置代理访问selenium server
          "proxyType": "manual",
          "httpProxy": "http://corporate.proxy:8080",
          "socksUsername": "codeceptjs",
          "socksPassword": "secret",
          "noProxy": "127.0.0.1,localhost"
                }
            }
    },

    "MyHelper": {   //配置自定义的helper
      "require": "./helpers/custom_helpers.js",  //配置自定义helper的文件位置
      "defaultHost": "http://mysite.com" //配置base url
    },

    "Mochawesome": {  //在测试失败时，给mochawesome报告加上screenshot
        "uniqueScreenshotNames": "true"
    }
  },

  "include": {  //配置actor和pageobject。这样才能在测试文件中进行引用
    "I": "./steps_file.js",
    "landingPage": "./pages/landingPage.js",   //配置Page Object的文件，可以让测试文件直接调用
    "landingPageStep": "./steps/LandingPage.js", //配置Step Object的文件，可以让测试文件直接调用
    "landingPageFragment": "./fragments/LandingPage.js" //配置Page Fragment的文件，可以让测试文件直接调用
  },

  "mocha": {   //mocha选项，可以配置reporter
  "reporterOptions": {
      "reportDir": "output",  //配置产生的测试报告文件放到output目录下，比如Mochawesome产生的文件
      "mochaFile": "output/result.xml"  //配置mocha-junit-reporter产生的xml文件放到output目录下
    }
  },

  "bootstrap": "./util/bootstrapAndTeardown/selenium-standalone-start.js", //配置bootstrap脚本的位置，让此脚本在全部测试运行之前运行
  "teardown": "./util/bootstrapAndTeardown/selenium-standalone-stop.js", //配置teardown脚本的位置，让此脚本在全部测试运行之后运行

  "hooks": ["./util/custom_hooks/event_listener.js"],  //配置自定义的hook文件来扩展codeceptjs

  "tests": "./spec/s*_test.js",   //配置需要运行的测试

  "timeout": 10000, //默认的测试超时时间

  "name": "codeceptjs-init",  //项目名称，not used

  "multiple": {    //配置run-multiple 并发多个测试套件的执行
    "smoke": {     //配置一个测试套件 smoke
      "grep": "@smoke",     //配置了该测试套件仅包含有@smoke标签的测试场景。如果没有定义，则代表所有测试场景
      "browsers": [   //配置可以在chrome和firefox中运行测试
        "chrome",
        "firefox"
      ]
    }
  }
}
```

#### 动态配置

###### override选项

通过选项`--override`或`-o`我们可以在运行测试的时候，修改相应的配置，做到运行时的动态配置。

`codeceptjs run -o '{ "helpers": {"WebDriverIO": {"browser": "firefox"}}}'`

###### config选项

通过选项`--config`或`-c`，我们可以运行指定的配置文件；这些文件可以是不同名字的或是在项目不同的目录下

`codeceptjs run --config=./path/to/my/config.json`

###### 使用codecept.conf.js

我们可以创建`codecept.conf.js`来包含更多动态的配置项。只需要export config属性即可。

```js
exports.config = {
  helpers: {
    WebDriverIO: {
      // load variables from the environment and provide defaults
      url: process.env.CODECEPT_URL || 'http://localhost:3000',

      user: process.env.CLOUDSERVICE_USER,
      key: process.env.CLOUDSERVICE_KEY,

      coloredLogs: true,
      waitForTimeout: 10000
    }
  },

  // don't build monolithic configs
  mocha: require('./mocha.conf.js') || {},

  // here goes config as it was in codecept.json
  // ....
};
```
###### process.profile and  --profile选项

通过在`codecept.conf.js`中配置`process.profile`，我们可以在运行测试时通过`--profile`制定相关的值写入配置文件，做到动态配置。

比如下面的列子中，`--profile firefox`会被作为`process.profile`的值写入配置文件.

`codeceptjs run --profile firefox`

```js
exports.config = {
  helpers: {
    WebDriverIO: {
      url: 'http://localhost:3000',
      // load value from `profile`
      browser: process.profile || 'firefox'

    }
  }
};
```

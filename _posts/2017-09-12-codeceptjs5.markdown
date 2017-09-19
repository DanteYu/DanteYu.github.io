---
layout:     post
title:      codeceptjs学习笔记
subtitle:   配置文件解释
date:       2017-09-12 12:00:00
author:     DanteYu
tags:
    - codeceptjs
---

这篇文章会介绍codeceptjs配置文件的基本用法

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

  "include": {
    "I": "./steps_file.js",
    "landingPage": "./pages/landingPage.js",   //配置Page Object的文件，可以让测试文件直接调用
    "landingPageStep": "./steps/LandingPage.js", //配置Step Object的文件，可以让测试文件直接调用
    "landingPageFragment": "./fragments/LandingPage.js" //配置Page Fragment的文件，可以让测试文件直接调用
  },

  "mocha": {
  "reporterOptions": {
      "reportDir": "output",  //配置产生的测试报告文件放到output目录下，比如Mochawesome产生的文件
      "mochaFile": "output/result.xml"  //配置mocha-junit-reporter产生的xml文件放到output目录下
    }
  },

  "bootstrap": "./util/bootstrapAndTeardown/selenium-standalone-start.js", //配置bootstrap脚本的位置，让此脚本在全部测试运行之前运行
  "teardown": "./util/bootstrapAndTeardown/selenium-standalone-stop.js", //配置teardown脚本的位置，让此脚本在全部测试运行之后运行

  "hooks": ["./util/custom_hooks/event_listener.js"],  //配置自定义的hook文件来扩展codeceptjs

  "tests": "./spec/s*_test.js",   //配置需要运行的测试

  "timeouts": {   // 配置超时时间。webdriverio的timeouts为键值对形式
  "script": 60000,
  "page load": 10000
  },

  "name": "codeceptjs-init",  //项目名称

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

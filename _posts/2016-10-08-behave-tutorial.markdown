---
layout:     post
title:      python behave之基本教程
subtitle:   ""
date:       2016-10-13 12:00:00
author:     DanteYu
tags:
    - behave
    - BDD
---


基本概念

behave要被执行，需要运行在满足下面两种情况的目录下

- 有feature files。这个feature files可以试由非技术人员编写

- 一个”steps’目录，steps里面包含python step implementation

还可以添加一些environmental controls。 比如before / after / scenarios / features / the whole shooting match

一个可执行的最小feature目录为
features/
features/everything.feature
features/steps/
features/steps/steps.py

最小为一个features目录 下面一个feature文件和一个steps目录。steps里面包含了测试代码。

一个更复杂的目录为
features/
features/signup.feature
features/login.feature
features/account_details.feature
features/environment.py
features/steps/
features/steps/website.py
features/steps/utils.py

按照项目的不同模块有不同的feature files，也有environment.py这样的环境配置文件
在steps下面 也有 测试代码

在命令行执行behave即可执行所有测试，在console能看到测试结果

什么是feature files

feature file是指一个通常命名为 **.feature的纯文本文件(UTF-8)。这个文件里面包含了用自然语言(Gherkin)描述的系统的功能特征。这些功能特征是具有代表性的期望结果。

Feature: Fight or flight
  In order to increase the ninja survival rate,
  As a ninja commander
  I want my ninjas to decide whether to take on an
  opponent based on their skill levels

  Scenario: Weaker opponent
    Given the ninja has a third level black-belt
     When attacked by a samurai
     Then the ninja should engage the opponent

  Scenario: Stronger opponent
    Given the ninja has a third level black-belt
     When attacked by Chuck Norris

     Then the ninja should run for his life

使用Gherkin来描述，具有以下特点：

- 结构是 feature下面有多个scenarios，scenarios下面是 GIVEN WHEN THEN的表述方法
- feature的描述用的是 in order to ….. as a ….. I want …….
- behave会采用 GIVEN WHEN THEN的步骤描述来map到测试代码中对应的相同步骤描述。GIVEN WHEN THEN是真实的执行步骤
- GIVEN： 在用户或是外部系统对应用进行交互之前，我们要把系统处于一个已知的状态。这个更加明确测试执行的前提条件和所要求的系统状态。要避免在GIVEN中涉及到用户交互
- WHEN：用户或是外部系统所采取的与待测试系统的交互步骤。这个交互能改变待测试系统的状态
- THEN： 待观察的结果或是期望结果

- 除了GIVEN WHEN THEN，我们还可以使用AND或是BUT来做为步骤从而进行步骤描述的扩展。

Scenario: Stronger opponent
  Given the ninja has a third level black-belt
   When attacked by Chuck Norris
   Then the ninja should run for his life

    And fall off a cliff

Scenarios Outlines来实现数据驱动

在behave实现数据驱动测试可以使用Scenario Outline这个关键字 配合 Examples这个关键字使用。 不同的数据会在相同的方法中执行。

Scenario Outline: Blenders
   Given I put <thing> in a blender,
    when I switch the blender on
    then it should transform into <other thing>

 Examples: Amphibians
   | thing         | other thing |
   | Red Tree Frog | mush        |

 Examples: Consumer Electronics
   | thing         | other thing |
   | iPhone        | toxic waste |
   | Galaxy Nexus  | toxic waste |

上面的例子中 使用了Scenario Outline来定义scenario 在GIVEN中使用 <varible name>来定义数据变量。
例子中有两个变量 <thing>和<other thing> 这两个变量名会在Examples中变为table的heading。Examples中的表格数据就是传入方法的数据

behave会运行表格中的每一行，每一行就代表着一个场景

Step Data 步骤数据

behave支持对feature文件中的step增加描述(Context.text)和表格(Context.table)来增加对feature的描述以及实现数据驱动

描述 Context.text - 在step后用两个“”“包含的文本
Scenario: some scenario
  Given a sample text loaded into the frobulator
     """
     Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do
     eiusmod tempor incididunt ut labore et dolore magna aliqua.
     """
 When we activate the frobulator
 Then we will find it similar to English
这个文本可以在测试代码中使用Context.text来调用

表格 Context.table - 在step后缩进表示的数据表格
Scenario: some scenario
  Given a set of specific users
     | name      | department  |
     | Barry     | Beer Cans   |
     | Pudey     | Silly Walks |
     | Two-Lumps | Silly Walks |

 When we count the number of people in each department
 Then we will find two people in "Silly Walks"
  But we will find one person in "Beer Cans"
这个表格的数据可以再测试代码中使用Context.table来调用，然后加到model里面。下面是example
@given('a set of specific users')
def step_impl(context):
    for row in context.table:
        model.add_user(name=row['name'], department=row['department'])

测试代码 - Python Step Implementations

测试代码实现在python文件中，这些python文件都需要被放入到”steps“文件夹下。
测试代码的文件名并不需要与feature文件的名称一致。

Steps是通过修饰符来进行匹配的。在测试代码中，修饰符接受一串字符串，这串字符串要和feature文件中scenario使用的字符串一样

feature文件中描述如下
Scenario: Search for an account
   Given I search for a valid account
    Then I will see the account details
则测试代码文件中应该如下

@given('I search for a valid account')
def step_impl(context):
   context.browser.get('http://localhost:8000/index')
   form = get_element(context.browser, tag='form')
   get_element(form, name="msisdn").send_keys('61415551234')
   form.submit()

@then('I will see the account details')
def step_impl(context):
   elements = find_elements(context.browser, id='no-account')
   eq_(elements, [], 'account not found')
   h = get_element(context.browser, id='account-head')
   ok_(h.text.startswith("Account 61415551234"),
       'Heading %r has wrong text' % h.text)

given when then 都不是必须的，都可以根据情况来使用

step修饰符即是 @given @then @when

”and“和”but“会被当做when/given/then的一部分来执行。比如一个and属于given step下，那么and这个step就会变为given的一部分

如果你希望你的step能够invoke另外一个step，可以使用Context.execute_steps()
@when('I do the same thing as before')
def step_impl(context):
    context.execute_steps('''
        when I press the big red button
         and I duck
    ''')

”I do the same thing as before“ step会执行另外两个step

Step Parameters

在feature文件中step的描述可以包含参数 step parameter以方便重用

比如下面的scenario中 Then都是使用了差不多的描述
Scenario: look up a book
  Given I search for a valid book
   Then the result page will include "success"

Scenario: look up an invalid book
  Given I search for a invalid book
   Then the result page will include "failure"

唯一的不同就是 “success” 和 “failure”。这个可以使用测试代码来处理
@then('the result page will include "{text}"')
def step_impl(context, text):
   if text not in context.response:
       fail('%r not in %r' % (text, context.response))

success和failure会作为text参数传入方法里面，然后会判断是否在context.response里面。context.response会在given step里面被加入一点text

这样的话 就可以多次使用同一个描述，实现step的重用

behave使用下面三种parsers

- parse
- cfparse
- re

Context

在behave的step中都有一个context参数。这个参数的作用是用于存储信息以及在不同的step中分享信息。context在given when then三个level中都会run，并且有behave自动管理

当随着测试执行，behave执行到了新的feature或是scenario，它给context加了a new layer，可以允许像增加新字段，或是overwrite以前定义好的。可以看成这是一个scopes

context可以在step中间分享信息
@given('I request a new widget for an account via SOAP')
def step_impl(context):
    client = Client("http://127.0.0.1:8000/soap/")
    context.response = client.Allocate(customer_first='Firstname',
        customer_last='Lastname', colour='red')

@then('I should receive an OK SOAP response')
def step_impl(context):
    eq_(context.response['ok'], 1)

given里面起了一个服务器，然后发送了请求，response赋予了context.response。在then中可以继续使用context.response来检查刚才的response里面有没有我们需要验证的值

除了context.response,还有context.text, context.table, context.failed可以使用

也可以自定义一些变量来传递
context.chrome = webdriver.Chrome()
context.server = simple_server.Server()

Environmental Controls

environment.py定义一些测试hook，比如在event before 或是 after 执行的方法

在feature level定义的environmental controls file会override scenario level；如果改变了scenario level是不会影响feature level

before_step(context, step), after_step(context, step)
These run before and after every step.
before_scenario(context, scenario), after_scenario(context, scenario)
These run before and after each scenario is run.
before_feature(context, feature), after_feature(context, feature)
These run before and after each feature file is exercised.
before_tag(context, tag), after_tag(context, tag)
These run before and after a section tagged with the given name. They are invoked for each tag encountered in the order they’re found in the feature file. Seecontrolling things with tags.
before_all(context), after_all(context)
These run before and after the whole shooting match.

The feature, scenario and step objects represent the information parsed from the feature file. They have a number of attributes:
上面方法的feature， scenario， step对象代表着从feature file解析过来的信息。这些对象有着下面的属性。我们可以在测试中使用这些属性信息
keyword
“Feature”, “Scenario”, “Given”, etc.
name
The name of the step (the text after the keyword.)
tags
A list of the tags attached to the section or step. See controlling things with tags.
filename and line
The file name (or “<string>”) and line number of the statement.

一个比较通用的用法是设置浏览器或是web server来运行你的测试

import threading
from wsgiref import simple_server
from selenium import webdriver
from my_application import model
from my_application import web_app

def before_all(context):
    context.server = simple_server.WSGIServer(('', 8000))
    context.server.set_app(web_app.main(environment='test'))
    context.thread = threading.Thread(target=context.server.serve_forever)
    context.thread.start()
    context.browser = webdriver.Chrome()

def after_all(context):
    context.server.shutdown()
    context.thread.join()
    context.browser.quit()

def before_feature(context, feature):

    model.init(environment='test')

Controlling Things With Tags

在behave中我们可以tag我们的feature文件。这就意味着我们能够选择的执行feature或是feature文件中的scenarios

如果我们有下面这样的feature 文件
Feature: Fight or flight
  In order to increase the ninja survival rate,
  As a ninja commander
  I want my ninjas to decide whether to take on an
  opponent based on their skill levels

  @slow
  Scenario: Weaker opponent
    Given the ninja has a third level black-belt
    When attacked by a samurai

    Then the ninja should engage the opponent
  @slow1
  Scenario: Stronger opponent
    Given the ninja has a third level black-belt
    When attacked by Chuck Norris
    Then the ninja should run for his life

如果我们执行 behave —tags=slow，只有被tag为slow的scenario  Weaker opponent被执行
如果我们执行 b2have —tags=-slow, 只有被tag为slow的scenario  Weaker opponent不被执行

如果我们执行behave —tags=slow,slow1 只要被tag为slow或slow1的scenario被执行
如果我们执行behave —tags=slow —tags=slow1 只要被tag为slow并且slow1的scenario被执行

tags与environment.py的互动：

- 如果一个feature或是scenario被skip了，那么相应的before_和after_都不会执行
- environment.py中各方法中的feature/scenario对象都有tags属性，这个属性是列出了所有tag名称的列表
- environment.py中的before_tag和after_tag。如果这两个方法被传入了”slow”，那么在执行被tag为slow的scenario之前，这两个方法会被调用

for example，部分scenario被tag为@browser，则我们可以使用feature.tags来查看tag有没有browser.这样做，我们可以指定哪些feature需要执行这个before_和after_

def before_feature(context, feature):
    model.init(environment='test')
    if 'browser' in feature.tags:
        context.server = simple_server.WSGIServer(('', 8000))
        context.server.set_app(web_app.main(environment='test'))
        context.thread = threading.Thread(target=context.server.serve_forever)
        context.thread.start()
        context.browser = webdriver.Chrome()

def after_feature(context, feature):
    if 'browser' in feature.tags:
        context.server.shutdown()
        context.thread.join()
        context.browser.quit()

Debug-on-Error (in Case of Step Failures)

当一个step失败的时候，behave提供”debug on error/failure”功能来帮助debug。这个功能会通过”after_step()"这个hook来实现

当需要使用这个功能的时候，我们可以通过命令行传入的配置数据来使其enable或是disable。

用户可以：

- 在命令行提供相关参数(定义好的userdata)

- 把参数值存储在”behave.userdata”

# -- FILE: features/environment.py
# USE: behave -D BEHAVE_DEBUG_ON_ERROR         (to enable  debug-on-error)
# USE: behave -D BEHAVE_DEBUG_ON_ERROR=yes     (to enable  debug-on-error)
# USE: behave -D BEHAVE_DEBUG_ON_ERROR=no      (to disable debug-on-error)

BEHAVE_DEBUG_ON_ERROR = False

def setup_debug_on_error(userdata):
    global BEHAVE_DEBUG_ON_ERROR
    BEHAVE_DEBUG_ON_ERROR = userdata.getbool("BEHAVE_DEBUG_ON_ERROR")

def before_all(context):
    setup_debug_on_error(context.config.userdata)

def after_step(context, step):
    if BEHAVE_DEBUG_ON_ERROR and step.status == "failed":
        # -- ENTER DEBUGGER: Zoom in on failure location.
        # NOTE: Use IPython debugger, same for pdb (basic python debugger).
        import ipdb
        ipdb.post_mortem(step.exc_traceback)
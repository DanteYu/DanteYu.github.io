---
layout:     post
title:      python behave
subtitle:   feature setup testing
date:       2016-10-20 12:00:00
author:     DanteYu
tags:
    - behave
    - BDD
---

### Feature Testing Layout

要让behave工作起来需要下面三种类型的文件

- 由非技术人员 BA/Sponsor等编写的 **feature files** ， 这些文件包含着应用的场景行为
- 一个 **step** 目录。这个目录下面包含着scenario对应的测试代码
- environmental control文件。这个是optional的
    - environment.py里面包含着关于`run` `before` `after` `steps` `scenarios` `features`等testing hook代码

一个可执行的最小feature目录为
```
features/
features/everything.feature
features/steps/
features/steps/steps.py
```
最小为一个features目录下面一个`feature`文件和一个`steps`目录。steps里面包含了测试代码。

一个更复杂的目录为
```
features/
features/signup.feature
features/login.feature
features/account_details.feature
features/environment.py
features/steps/
features/steps/website.py
features/steps/utils.py
```

### behave命令执行

behave是灵活的，我们通过执行带有不同选项的命令来帮助behave找到feature文件，然后来执行指定的文件

- 当执行`behave` 时， behave会从当前目录下寻找”features”子目录，然后从中读取feature文件
- 当执行`behave ./features`，给behave传递features目录所在路径，behave会从中读取至少一个 __name.feature__ 文件和一个叫 __steps__ 的目录。对于environment.py, 如果有的话，必须是和 __steps__ 目录在同一个目录下
- 当执行`behave ./features/BookInformationAPITest.feature` 给behave传递了一个指定的feature file，behave就会执行指定的这个feature
- 也可以给behave传递a directory containing your feature files。 behave的工作方式和上述方法类似

`behave -v`   (-v verbose)可以看到更详细的behave行为

### Gherkin: Feature Testing Language

behave的features文件是由一种叫Gherkin的语言编写的并且被命名为 __name.feature__

在理想情况下，这些feature files是有项目内的非技术人员比如需求方使用自然语言来编写。feature files有两个目的，一个是**作为正式文档** ，二是**支持自动化测试**

feature files的编写非常自由灵活，但是还是有一些好的准则可以坚持：

- 行结尾终止声明
- spaces或tabs都可以作为缩进
- 缩进都会被忽略，但是缩进是一个用来展示文本结构的好办法
- 大部分的行要以关键字为开头。Keywords: Feature, Scenario, Given

### 在文件的任何地方都可以使用 “#”来进行注释

### Features

Features是由Scenarios来组成。Feature还可以加上description，background和tag，不过这些都是可选的。

最简单的feature形式为
```
Feature: feature name

  Scenario: some scenario
      Given some condition
       Then some result is expected.
```

把description background tag都加上后，feature形式为
```
@tags @tag
Feature: feature name
  description
  further description

  Background: some requirement of this test
    Given some setup condition
      And some other setup action

  Scenario: some scenario
      Given some condition
       When some action is taken
       Then some result is expected.

  Scenario: some other scenario
      Given some other condition
       When some action is taken
       Then some other result is expected.
```
Feature name应该为待测feature的合理描述的标题

#### description
description是optional的，可以用来澄清任何潜在的confusions或是scope issue

#### Background
background是由一些step组成，这些step和scenarios里面的一样。Background允许你给feature中的scenario加一些context。Background的执行是在feature文件中所有的scenario之前，不过是在所有before hooks之后。

一个feature文件只有一个background。ackground必须在scenario或是scenario outline之前定义好。background经常被用来做一些setup的事情，比如登陆一个页面或是设置好数据库


使用background的好实践：

- 不要用”Background’来设置较为复杂的状态除非客户要求
- 保持”Background”短小精炼。如果Background大于4行，我们就可以把一些无关的细节挪到high level的steps
- 使得“Background”栩栩如生，通俗易懂。可以使用丰富的名称和描述来讲述一个故事。
- 保持scenario不会很长，也不能太多。不然Background会执行很多遍

#### Scenarios

Scenarios描述了需要测试的产品行为。Scenario是由一系列steps组成。这些steps典型的形式是”given some condition” “then we expect some test will pass"

scenario最简单的形式为
```
Scenario: we have some stock when we open the store
  Given that the store has just opened
  Then we should have items for sale.
```

如果有需要的话，and或是but都可以加到scenario里面，放在given when then之后
```
Scenario: Replaced items should be returned to stock
  Given that a customer buys a blue garment
    and I have two blue garments in stock
    but I have no red garments in stock
    and three black garments in stock.
   When he returns the garment for a replacement in black,
   then I should have three blue garments in stock
    and no red garments in stock,
    and two black garments in stock.
```
一个比较好的实践是一个scenario就测试一个行为或是一种desired outcome

### Scenario Outlines

当对于同一个场景有着很多不同的期待输入条件和输出结果，我们可以使用scenario outline来做数据驱动，这样可以重用scenario中的step

一个outline包含定义在step中的关键字，这些关键字会被表格中的数据来代替。一个scenario outline可以有多个example tables

```
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
```

behave会跑example table里面的每一行。每一行的数据会去替代step里面尖括号定义的<thing> <other thing>

注意尖括号里的名字必须和表格头的名字一致。

python step implementation如下
```python
from behave   import given, when, then
from hamcrest import assert_that, equal_to
from blender  import Blender

@given('I put "{thing}" in a blender')
def step_given_put_thing_into_blender(context, thing):
    context.blender = Blender()
    context.blender.add(thing)

@when('I switch the blender on')
def step_when_switch_blender_on(context):
    context.blender.switch_on()

@then('it should transform into "{other_thing}"')
def step_then_should_transform_into(context, other_thing):
    assert_that(context.blender.result, equal_to(other_thing))
```

把<thing>作为参数传入方法里面

### Steps

每个step都会占据一行。并且以下面五个关键字中的一个来开头：

- given
- when
- then
- and
- but

通常来讲这些关键字都应该是Title Case就是首字母大写的形式，部分语言还是支持只有全小写

step的描述不应该包含太多关于测试机制的细节

比如下面
> Given a browser client is used to load the URL "http://website.example/website/home.html"

就应该变为
> Given we are looking at the home page

Steps的实现是由python写的测试代码组成，python的模块“name.py”会被放在”steps”文件夹下。python模块的文件命名是没有规定的。所有在”steps”下面的文件都会被behave引入

#### Given
givens的目的是在用户或外部系统对系统进行交互之前，使系统处于一个可知的状态。在givens中要避免涉及用户交互的操作。
在use case中，givens可以被理解为前置条件precondition。也可以在Given中使用table来设置一些数据

#### When
whens的目的是描述用户或是外部系统施展的关键行为。这个交互行为会引起系统状态的变化

#### Then
thens的目的是观察结果。这个观察的结果应该与feature描述的用户价值business value/benefit相关联。这个结果也应该是一个系统产生的

具体的结果而不是对用户不可见，埋藏得很深的结果。对用户不可见的结果也许会没有business value。比如我们应该多验证用户看得见的结果而不是去数据库里面找结果

#### 我们还可以使用And But来组织steps

```
Scenario: Multiple Givens
  Given one thing
  Given an other thing
  Given yet an other thing
   When I open my eyes
   Then I see something
   Then I don't see something else
```
可以写为

```
Scenario: Multiple Givens
  Given one thing
    And an other thing
    And yet an other thing
   When I open my eyes
   Then I see something
    But I don't see something else
```

这两种写法对behave都是一样的

### Step Data

Steps除了`given` `when` `then`，还允许包含像文本`text`和表格`table`样的数据。所以一个`step`可以包含自己的`text`和`table`，这就是Step Data

Scenario Outline values的一个替代方法就是使用\<name>在Step data text或是table中

#### Text
任何在step中使用三个双引号包括的文本就是Step data中的text

```
Scenario: some scenario
  Given a sample text loaded into the frobulator
     """
     Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do
     eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
     enim ad minim veniam, quis nostrud exercitation ullamco laboris
     nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in
     reprehenderit in voluptate velit esse cillum dolore eu fugiat
     nulla pariatur. Excepteur sint occaecat cupidatat non proident,
     sunt in culpa qui officia deserunt mollit anim id est laborum.
     """
 When we activate the frobulator
 Then we will find it similar to English
```
注意非第一行的whitespace数不能比第一行少。

在测试代码中，我们使用`Context.text`来获取这段text。`Context.text`也可以在每一个step function中传递使用。

第一个step定义的text是可以在scenario后面的step中的context中使用。在候选step中定义的text会overwrite之前step中的text。

#### Table
我们可以把一个表格数据赋予给一个step，只需要通过简单的输入，缩进即可。当我们想把一系列的数据赋值给一个model
的时候，这个Table特别有用

整个表格有两个 vertical bar “|“ 来区别列。任何在列和vertical bar之间的whitespace会被删除
```
Scenario: some scenario
  Given a set of specific users
     | name      | department  |
     | Barry     | Beer Cans   |
     | Pudey     | Silly Walks |
     | Two-Lumps | Silly Walks |

 When we count the number of people in each department
 Then we will find two people in "Silly Walks"
  But we will find one person in "Beer Cans"
```
在测试代码中，我们使用`Context.table`来获取这段table。`Context.table`也可以在每一个step function中传递使用。测试代码中，我们使用如下
```python
@given('a set of specific users')
def step_impl(context):
    for row in context.table:
        model.add_user(name=row['name'], department=row['department'])
```

### Tags

feature file中可以使用tag功能。tag功能可以让behave实现选择性的执行feature

tag可以作用于feature，scenario和scenario outline。作用于feature的tag也会作用于feature下的scenario或scenario outlines

tag使用修饰符的样式作用于feature/scenario/scenario outlines，多个tag之间可以使用whitespace来进行间隔

For example:
```
@wip @slow
Feature: annual reporting
  Some description of a slow reporting system.
```
or

```
@wip
@slow
Feature: annual reporting
  Some description of a slow reporting system.

Controlling Your Test Run With Tags 我们可以使用tag来选择性的执行测试
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

  Scenario: Stronger opponent
    Given the ninja has a third level black-belt
    When attacked by Chuck Norris
    Then the ninja should run for his life
```
对于上面这个feature file，

* 如果运行 `behave —tags=slow` 就只会运行标记了slow的Scenario Weaker opponent
* 如果运行 `behave —tags=-slow`，就会运行没有标记slow的Scenarios

一个常用的场景是使用`@wip`来处理正在开发中的scenario或feature，使用`behave —tags=wip`来运行这个正在开发的feature

在命令行中tag的用法：
* 如果我们执行behave —tags=slow,slow1 只要被tag为slow或slow1的scenario被执行
* 如果我们执行behave —tags=slow —tags=slow1 只要被tag为slow并且slow1的scenario被执行

#### tags与environment.py的互动

- 如果一个feature或是scenario被skip了，那么相应的before_和after_都不会执行
- environment.py中各方法中的feature/scenario对象都有tags属性，这个属性是列出了所有tag名称的列表
- environment.py中的before_tag和after_tag。如果这两个方法被传入了”slow”，那么在执行被tag为slow的scenario之前，这两个方法会被调用

for example，部分scenario被tag为@browser，则我们可以使用feature.tags来查看tag有没有browser.这样做，我们可以指定哪些feature需要执行这个before_和after_
```python
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
```
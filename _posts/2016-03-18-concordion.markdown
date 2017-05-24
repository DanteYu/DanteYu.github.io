---
layout:     post
title:      concordion学习笔记
subtitle:   ""
date:       2016-10-13 12:00:00
author:     DanteYu
tags:
    - concordion
    - BDD
---

1. concordion活文档
自然语言，易读
包含很多examples
文档是html，所以构建出可以navigable structure的文档
频繁的运行可以使文档和系统功能同步。如果系统行为发生了变化，相应的文档就会fail

2. concordion文档不包含实现细节，把需求和实现分开，包含列子，实现方式可变，方便重构

3. concordion是Junit的扩展

4. 易学，基于specifications by example、

5. 不需要特定的语言格式，使用自然语言，关注易读性

6. 丰富的测试报告。 HTML格式的报告可以加入超链接和图片等多种

7. 文档可以当成unit test来运行。具备和Junit测试一样的深度

8. BA QA DEV可以一起合作写出文档

9. 文档可以适用于不同级别 unit， component， subsystem， system

10. 活文档可以用来定义story的验收条件，帮助PO来控制项目范围

11. 提高测试覆盖率。不管实现如何，需求一定会被满足

一个concordion规格文档由两部分组成

- 描述功能的html文件

- 测试代码 (基于Junit)

两个文件应该在同一个文件夹下

为了两个文档的融合使用，html里面应该包含concordion的相关命令。这个命令已结点的属性形式出现

- <html xmlns:concordion="http://www.concordion.org/2007/concordion">
-     <body>
        <p concordion:assertEquals="getGreeting()">Hello World!</p>
-     </body>
</html>

测试代码基于Junit，方法名需要和html中的名字一样，使用ConcordionRunner和Junit的RunWith来让concordion把相关的命令和类联系起来

- package example;

- import org.concordion.integration.junit4.ConcordionRunner;
- import org.junit.runner.RunWith;

- @RunWith(ConcordionRunner.class)
public class HelloWorldFixture {

-     public String getGreeting() {
-         return "Hello World!";
-     }
}

测试报告输出默认目录是

system property java.io.tmpdir.

concordion:assertEquals  验证执行方法的返回值和所期待的结果一致

concordion:set 定义一个变量
        <p>
            The greeting for user <span concordion:set="#firstName">Bob</span>
            will be:
            <span concordion:assertEquals="greetingFor(#firstName)">Hello Bob!</span>
        </p>

concordion:execute 可以执行一个测试代码里面的方法

- 执行一个没有返回值的方法。

    - 一般来说，执行一个没有返回值的方法，要用set或是setUp开头。
        <p>
            If the time is
            <span concordion:set="#time">09:00AM</span>
    -             <span concordion:execute="setCurrentTime(#time)" />
    -             then the greeting will say:
            <span concordion:assertEquals="getGreeting()">Good Morning World!</span>
    -         </p>

    - 利用#TEXT这个特殊变量可以简写上面的spec。可以不适用concordion:set。在把#TEXT作为参数传给方法，后面接着#TEXT的值
        <p>
            If the time is
            <span concordion:execute="setCurrentTime(#TEXT)">09:00AM</span>
    -             then the greeting will say:
            <span concordion:assertEquals="getGreeting()">Good Morning World!</span>
        </p>

- 执行一个有返回值的方法

    - 可以使用方法接受返回值
            <p>
                The full name
                <span concordion:execute="#result = split(#TEXT)">John Smith</span>
    -                 will be broken into first name
                <span concordion:assertEquals="#result.firstName">John</span>
    -                 and last name
                <span concordion:assertEquals="#result.lastName">Smith</span>.
    -             </p>

                   这里的#result是一个类,split()方法返回一个类对象. firstName and lastName 是类属性

- 处理不寻常的语句结构

    - 当我们面对规格文档描述不利于concordion命令的实现的时候，我们可以把execute命令放到更上一层结点结构当中
<p concordion:execute="#greeting = greetingFor(#firstName)">
    -     The greeting "<span concordion:assertEquals="#greeting">Hello Bob!</span>"
    -     should be given to user <span concordion:set="#firstName">Bob</span>
    -     when he logs in.
</p>

                          execute命令的执行是有一定的顺序的
                              首先是处理所有child的set命令
                              然后是自己的命令
                              然后是自己child的execute命令
                              最后是所有child的assertEquals方法

concordion:execute on a <table>  可以设置在table heading级别来执行table里面的每行数据

当我们需要表现同一个行为的不同列子的时候，需要重复同样的语句结构。这种情况下可以使用table来表达

            <table>
                <tr>
                    <th>Full Name</th>
                    <th>First Name</th>
                    <th>Last Name</th>
                </tr>
                <tr concordion:execute="#result = split(#fullName)">
                    <td concordion:set="#fullName">John Smith</td>
                    <td concordion:assertEquals="#result.firstName">John</td>
                    <td concordion:assertEquals="#result.lastName">Smith</td>
                </tr>
                <tr concordion:execute="#result = split(#fullName)">
                    <td concordion:set="#fullName">David Peterson</td>
                    <td concordion:assertEquals="#result.firstName">David</td>
                    <td concordion:assertEquals="#result.lastName">Peterson</td>
                </tr>
            </table>

但是这样看起来也是比较重复的

所以concordion支持把concordion命令放在table heading级别，每一个td就会重复th的命令

下面的列子中，execute在table级别，set  assertEquals在table heading级别

            <table concordion:execute="#result = split(#fullName)">
                <tr>
                    <th concordion:set="#fullName">Full Name</th>
                    <th concordion:assertEquals="#result.firstName">First Name</th>
                    <th concordion:assertEquals="#result.lastName">Last Name</th>
                </tr>
                <tr>
                    <td>John Smith</td>
                    <td>John</td>
                    <td>Smith</td>
                </tr>
                <tr>
                    <td>David Peterson</td>
                    <td>David</td>
                    <td>Peterson</td>
                </tr>
            </table>

concordion:execute on a <list> 可以给方法传递有层级的测试数据

当我们使用concordion在列表上的时候，比如ol 和 ul 元素， 该execute命令会执行在每一个列表元素上。
这个特性能帮我们设置有层级结构的测试数据

<ol concordion:execute="parseNode(#TEXT, #LEVEL)">
    <li>Europe</li>
    <ul>
        <li>Austria</li>
        <ol>
            <li>Vienna</li>
        </ol>
        <li>UK</li>
        <ul>
            <li>England</li>
            <li>Scotland</li>
        </ul>
        <li>France</li>
    </ul>
    <li>Australia</li>
</ol>

当执行到<ol concordion:execute="parseNode(#TEXT, #LEVEL)”>时，给parseNode穿的参数就等于下面的表格

#TEXT

#LEVEL

Europe

1

Austria

2

Vienna

3

UK

2

England

3

Scotland

3

France

2

Australia

1

concordion:verifyRows     可以取出方法返回的可迭代的数据，并一一取出做验证。

当我们需要处理方法返回的大量数据内容，并且这些数据是可迭代的时候，我们可以使用concordion:verifyRows。 比如处理一个方法返回值是列表，而我们需要对列表里面的每一个值进行验证

    <table concordion:execute="setUpUser(#username)">
        <tr><th concordion:set="#username">Username</th></tr>
        <tr><td>john.lennon</td></tr>
        <tr><td>ringo.starr</td></tr>
        <tr><td>george.harrison</td></tr>
        <tr><td>paul.mccartney</td></tr>
    </table>

    <p>Searching for "<b concordion:set="#searchString">arr</b>" will return:</p>

    <table concordion:verifyRows="#username : getSearchResultsFor(#searchString)">
        <tr><th concordion:assertEquals="#username">Matching Usernames</th></tr>
        <tr><td>george.harrison</td></tr>
        <tr><td>ringo.starr</td></tr>
    </table>

verifyRows的语法是： var : iteration_object
#loopVar : expression

expression是一个可迭代对象并且对象里面的元素对象顺序是已知的
#loopVar能够访问返回迭代对象的每一个元素，并且支持使用assertEquals去进行验证
从expression返回的对象元素的顺序必须与行里设置的测试数据是一致的

注解

当我们把未完成的测试代码加入到我们的build当中，为了避免使build失效，我们可以加注解。

- @ExpectedToPass    期待pass
- @ExpectedToFail      期待fail
- @Unimplemented      未完成

For example:

import org.concordion.api.ExpectedToFail;
import org.concordion.integration.junit4.ConcordionRunner;
import org.junit.runner.RunWith;

@ExpectedToFail
@RunWith(ConcordionRunner.class)
public class GreetingTest {

  public String greetingFor(String firstName) {
        return "TODO";
  }
}




Fail-Fast

当出现异常的时候，concordion会继续执行剩下的测试来展示所有的问题。使用Fail-Fast可以使concordion遇到第一个异常时就停止继续执行，跳出测试。

方法是使用注解

Fail-Fast

Fail-Fast有一个参数onExceptionType,这个参数是一个列表，列表里面的内容是各种异常类型，只有在执行当中遇到列表里面的异常类型，Fail-Fast才会生效。
import org.concordion.api.FailFast;
import org.concordion.integration.junit4.ConcordionRunner;
import org.junit.runner.RunWith;

@FailFast(onExceptionType={DatabaseUnavailableException.class, IOException.class})
public class MyDataTest {

   public void connectToDatabase() {
        ....
   }
}

concordion:run 可以使该文档link到其他文档，并且run

concordion:assertTrue 验证执行方法返回值是不是True
concordion:assertFalse 验证执行方法返回值是不是False

<p>
    When user <b concordion:set="#firstName">Bob</b>
    logs in, the greeting will be:
    <b concordion:assertEquals="greetingFor(#firstName)">Hello Bob!</b>
</p>

<p>
    The first name <span concordion:assertTrue="#firstName.startsWith(#letter)">starts
    with <b concordion:set="#letter">B</b></span>.
</p>

<p>
    The first name <span concordion:assertTrue="#firstName.startsWith(#letter)">starts
    with <b concordion:set="#letter">C</b></span>.
</p>

支持Junit4.5以上

测试代码使用ConcordionRunner
package example;

import org.concordion.integration.junit4.ConcordionRunner;
import org.junit.runner.RunWith;

@RunWith(ConcordionRunner.class)
public class HelloWorldFixture {

    public String getGreeting() {
        return "Hello World!";
    }
}

返回值的类型 也可以是map

    public Map split(String fullName) {
        String[] words = fullName.split(" ");
        Map<String, String> results = new HashMap<String, String>();
        results.put("firstName", words[0]);
        results.put("lastName", words[1]);
        return results;
    }

可以使用MultiValueResult来返回多个结果。 MultiValueResult 是concordion api的一个类

package example;

import org.concordion.api.MultiValueResult;
import org.concordion.integration.junit4.ConcordionRunner;
import org.junit.runner.RunWith;

@RunWith(ConcordionRunner.class)
public class SplittingNamesTest {

    public MultiValueResult split(String fullName) {
        String[] words = fullName.split(" ");
        return multiValueResult()
                .with("firstName", words[0])
                .with("lastName", words[1]);
    }
}

使用这个方法，我们可以在测试代码里面设置比如属性firstName的值，然后在specification里面直接调用

                <span concordion:assertEquals="#result.firstName">John</span>

特殊变量 #HREF

当我们想使用外部文件作为测试数据时，可以使用#HREF指向一个link，这个link就是数据源文件。

比如我们把测试数据放在一个csv文件里面，我们就可以在html中写一个link，然后在这个<a>中使用concordion命令和#HREF

<a href="blah.csv" concordion:execute="#x = myMethod(#HREF)">test file</a>

<span concordion:assertEquals="#x">blah.csv</span>

Write specifications, not scripts

只在Specification中描述feature和功能，不要有太详细的步骤，具体的实现细节都应该放在测试代码里面。

Specification可以说是high level的测试脚本

concordion:echo  is able to print the variable to test report

  <div class="example">

<h3>Example</h3>

      <p>
          When user <b concordion:set="#firstName">World</b>
          logs in, the greeting will be:
          <b concordion:assertEquals="greetingFor(#firstName)">Hello, World!</b>
      </p>

      <p>
          Username: <span concordion:echo="#firstName" />  # this is the variable created in specification file
      </p>

  </div>

  <div class="example">

<h3>Example</h3>

      <p>
          When user <b concordion:set="#firstName">World</b>
          logs in, the greeting will be:
          <b concordion:assertEquals="greetingFor(#firstName)">Hello, World!</b>
      </p>

      <p>
          Username: <span concordion:echo="username" /> # this is the class variable from test fixture code
      </p>

  </div>

@RunWith(ConcordionRunner.class)
public class HelloWorldFixture {

    public String username = "username";

    public String greetingFor(String firstName) {
        return new Greeter().greetingFor(firstName);
    }
}
---
layout:     post
title:      安全测试实践
subtitle:   ""
date:       2021-02-11 12:00:00
author:     DanteYu
tags:
    - security
---

此文章记录我在平时做功能测试(故事卡测试)时，在安全方面有哪些设计和执行来覆盖最常见的一些安全漏洞

#### XSS(cross site scripting)
##### 如何测试
##### 如何修复和预防

#### SQL注入(SQL injection)
##### 如何测试
SQL注入需要输入含有恶意的SQL语句带入数据库执行，通过这个恶意的SQL语句，获取表数据以及对数据增查改删，因为SQL注入漏洞的危害是极大的。 推荐下面二种方法进行测试

第一种 手工测试
如何判断SQL注入是否存在--> 最为经典的单引号判断法：在参数后面加上单引号,比如: `http://xxx/abc.php?id=1'` 如果页面返回错误，则存在 Sql 注入。原因是无论字符型还是整型都会因为单引号个数不匹配而报错。

判断注入漏洞类型总结如下

1.对于数字 使用 `100` 判断查询正常, `100 and 1=1` 判断查询正常, `100'` 判断页面异常或是查询为空， `100 and 1=2` 查询数据为空
2.对于字符串 使用 `x` 判断查询正常, `x' and '1'='1` 判断查询正常, `x'` 判断页面异常或是查询为空， `x' and '1'='2` 查询数据为空
3.对于搜索 使用 `x` 判断查询正常, `x%' and '%'='` 判断查询正常, `x'` 判断页面异常或是查询为空， `x%' and 'a%'='b` 查询数据为空

通过上面的方式可以确定SQL注入是否存在，存在之后，就可以结合其他sql语句进行攻击。对于我们测试人员，在已知数据库结构和数据的情况下，就非常简单了，比如使用 `union select` 联合查询继续获取信息
`1' order by 1#`;(按照Mysql语法，#后面会被注释掉，使用这种方法屏蔽掉后面的单引号，避免语法错误)  不断增加 order by 后面的数字会判断出有多少列
* 输入`1' union select database(),user()#` 就等于`SELECT first_name, last_name FROM users WHERE user_id = '1' union select database(),user()#`;`  database()将会返回当前网站所使用的数据库名字. user()将会返回执行当前查询的用户名. 类似地还有 `1' union select table_name,table_schema from information_schema.tables where table_schema= 'dvwa'#` information_schema是mysql自带的一张表，这张数据表保存了Mysql服务器所有数据库的信息,如数据库名，数据库的表，表栏的数据类型与访问权限等


第二种 使用工具sqlmap

sqlmap 是一个开源的渗透测试工具，可以用来自动化的检测，利用SQL注入漏洞，获取数据库服务器的权限。它具有功能强大的检测引擎,针对各种不同类型数据库的渗透测试的功能选项，包括获取数据库中存储的数据，访问操作系统文件甚至可以通过外带数据连接的方式执行操作系统命令。

基本命令
* `python sqlmap.py -u 'http://mytestsite.com/page.php?id=5'`  sqlmap会对id参数使用不同的注入方法进行测试
* `python sqlmap.py -u 'http://mytestsite.com/page.php?id=5' --dbs` 获取数据库信息
* `python.exe sqlmap/sqlmap.py -u http://aa.com/wcms/show.php?id=3 -D cms -T cms_users -C username,password --dump` 指定库名表名字段列出指定字段

作者：taiji1985
链接：https://www.jianshu.com/p/65d7522ecc1f
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

可以参考的资料
[使用教程1](https://www.jianshu.com/p/65d7522ecc1f)
[使用教程2](https://www.freebuf.com/sectool/164608.html)
[使用教程3](https://www.jianshu.com/p/4fb15a2c9040)
[使用教程4](https://www.jianshu.com/p/c2b31bd04eeb)

*一般推荐使用sqlmap进行测试*。


##### 如何修复和预防
1. 当操作数据库时，需要实现PreparementStatement接口对SQL语句进行预编译处理。这种采用sql语句预编译和绑定变量，是防御sql注入的最佳方法。其原因就是：采用了PreparedStatement，就会将sql语句：”select id, no from user where id=?” 预先编译好，也就是SQL引擎会预先进行语法分析，产生语法树，生成执行计划，也就是说，后面你输入的参数，无论你输入的是什么，都不会影响该sql语句的语法结构了，因为语法分析已经完成了，而语法分析主要是分析sql命令，比如 select ,from ,where ,and, or ,order by 等等。所以即使你后面输入了这些sql命令，也不会被当成sql命令来执行了，因为这些sql命令的执行， 必须先的通过语法分析，生成执行计划，既然语法分析已经完成，已经预编译过了，那么后面输入的参数，是绝对不可能作为sql命令来执行的，只会被当做字符串字面值参数。所以sql语句预编译可以防御sql注入。实际项目中，一般我们都是采用各种的框架，比如ibatis, hibernate,mybatis等等。他们一般也默认就是sql预编译的
2. 如果必须要用字符串拼接，也可以对输入参数进行数据类型检查
3. 可以使用一些安全函数 ESAPI.encoder().encodeForSQL



#### 权限管理
##### 如何测试
* 越权 - 不同的用户应该有严格的权限管理，不被允许的资源就应该有相应的处理，比如通过URL访问不属于自己权限内的资源，就应该进入相应报错页面。一般这类处理都应该在需求阶段提出。越权一般分为水平越权和垂直越权。水平越权指得是同级别用户的越权，垂直越权指得是具有从属关系，从而带来不同权限范围的用户的越权
##### 如何修复和预防

#### 敏感数据
##### 如何测试
* 如果项目需求涉及图片上传，取名等时，我们就应该考虑对文字和图片进行风险控制，避免涉及暴力、政治和色情等不良信息在系统中存储使用。一般做法是对接第三方平台进行验证
* 如果项目中有涉及用户密码等隐私数据，我们就应该采用加密存储

#### 其他测试点
* 修改URL的参数
* 修改cookie中的数据



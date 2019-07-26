---
layout:     post
title:      Rest Assured使用tips
subtitle:   
date:       2019-07-10 12:00:00
author:     DanteYu
tags:
    - TestStrategy
---
这篇文章记录一些使用Rest Assured时遇到的问题以及对应的解决方案

##### json response是列表
当response是一个列表形式时，我们可以直接使用`$`或者`空字符串`作为路径来识别
```
when().
     get("/json").
then().
     body("$", hasItems(1, 2, 3)); // An empty string "" would work as well
```

##### 复杂的response解析
我们可以利用`groovy`语言优点，对response进行带有逻辑的解析

比如找到符合某个条件的值
```
when().
       get("/store").
then().
       body("store.book.findAll { it.price < 10 }.title", hasItems("Sayings of the Century", "Moby Dick"));
```

也可以写为
```
// Get the response body as a String
String response = get("/store").asString();
// And get all books with price < 10 from the response. "from" is statically imported from the JsonPath class
List<String> bookTitles = from(response).getList("store.book.findAll { it.price < 10 }.title");
```
对某个值进行求和
```
首先我们通过(store.book.author)得到了所有的author字段值，然后使用闭包里的方法{ it.length() }解析这个集合。

它所做的是对列表里的每一个author字段执行一次length()方法，然后返回一个新的列表。在这个列表中，我们再调用sum()方法来求得字符长度的总和。
when().
       get("/store");
then().
       body("store.book.author.collect { it.length() }.sum()", greaterThan(50));
```
Groovy有一个便利的方法可以遍历列表中的所有元素，使用*来调用
```
when().
       get("/store");
then().
       body("store.book.author*.length().sum()", greaterThan(50)).
```

```
// Get the response body as a string
String response = get("/store").asString();
// Get the sum of all author length's as an int. "from" is again statically imported from the JsonPath class
int sumOfAllAuthorLengths = from(response).getInt("store.book.author*.length().sum()");
// We can also assert that the sum is equal to 53 as expected.
assertThat(sumOfAllAuthorLengths, is(53));
```

##### 比较float
```
get("/price").then().assertThat().body("price", equalTo(12.12f));

```

##### 路径参数
```
given().
        pathParam("hotelId", "My Hotel").
        pathParam("roomNumber", 23).
when().
        post("/reserve/{hotelId}/{roomNumber}").
then().
         ..
```

```
given().
        pathParam("hotelId", "My Hotel").        
when().
        post("/reserve/{hotelId}/{roomNumber}", 23).
then().
         ..
```

##### 上传文件
```
given().
        multiPart(new File("/path/to/file")).
when().
        post("/upload");
```

##### 模式复用
与其复制一份响应的断言或者请求参数（的代码）到另一个测试用例中，我们可以使用 RequestSpecBuilder或者ResponseSpecBuilder定义一个规范提案。

```
ResponseSpecBuilder builder = new ResponseSpecBuilder();
builder.expectStatusCode(200);
builder.expectBody("x.y.size()", is(2));
ResponseSpecification responseSpec = builder.build();

// Now you can re-use the "responseSpec" in many different tests:
when().
       get("/something").
then().
       spec(responseSpec).
       body("x.y.z", equalTo("something"));
```

```
RequestSpecBuilder builder = new RequestSpecBuilder();
builder.addParam("parameter1", "parameterValue");
builder.addHeader("header1", "headerValue");
RequestSpecification requestSpec = builder.build();

given().
        spec(requestSpec).
        param("parameter2", "paramValue").
when().
        get("/something").
then().
        body("x.y.z", equalTo("something"));
```

##### 使用根路径

```
when().
        get("/something").
then().
         root("x.y"). // You can also use the "root" method
         body("firstName", is(..)).    //x.y.firstName
         body("lastName", is(..)).
         body("age", is(..)).
         body("gender", is(..));
```

```
when().
         get("/jsonStore").
then().
         root("store.category").
         body("size()", equalTo(4)).
         detachRoot("category").
         body("size()", equalTo(1));
```
在根路径上附加一些参数也很有用。
```
when().
         get("/jsonStore").
then().
         root("store.%s", withArgs("book")). //%s is book
         body("category.size()", equalTo(4)).
         appendRoot("%s.%s", withArgs("author", "size()")).
         body(withNoArgs(), equalTo(4));
```


##### 路径参数

在预定义的路径包含变量时，路径参数会很有用

```
String someSubPath = "else";
int index = 1;
get("/x").then().body("something.%s[%d]", withArgs(someSubPath, index), equalTo("some value")). ..  // something.else[0]
```

##### 复用sessionID
```
SessionFilter sessionFilter = new SessionFilter();

given().
          auth().form("John", "Doe").
          filter(sessionFilter).
when().
          get("/formAuth").
then().
          statusCode(200);


given().
          filter(sessionFilter). // Reuse the same session filter instance to automatically apply the session id from the previous response
when().
          get("/x").
then().
          statusCode(200);
```

##### 关闭url编码

在有些用例中仍然需要关闭URL编码的功能。其中一个原因是在使用rest-assured之前可能你已经做过一次编码。为防止两次URL编码您需要告诉rest-assured禁用这个功能。

```
String response = given().urlEncodingEnabled(false).get("https://jira.atlassian.com:443/rest/api/2.0.alpha1/search?jql=project%20=%20BAM%20AND%20issuetype%20=%20Bug").asString();
..
```

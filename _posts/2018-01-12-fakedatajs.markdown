---
layout:     post
title:      Fake data的使用和产生
subtitle:   JavaScript篇
date:       2018-01-12 12:00:00
author:     DanteYu
tags:
    - TestData
---
接上篇[Fake data文章](https://danteyu.github.io/2018/01/10/fakedatapython/)。本文主要介绍基于JavaScript的fake data generator。

#### API https://randomuser.me/

[randomuser.me](https://randomuser.me/)提供一个API`https://randomuser.me/api/`来返回随机用户数据，免费且十分方便好用。下面我们来看如何使用这个API。

##### 多格式

简单的一个GET请求`https://randomuser.me/api/`即可获得包含用户数据的response，默认格式是JSON，但通过`?format=xxx`参数我们可以指定不同的数据格式，目前支持的有
* JSON (default)
* PrettyJSON or pretty
* CSV
* YAML
* XML

结合[chakram](http://dareid.github.io/chakram/)我们来看看这个API如何使用
```js
var chakram = require('./node_modules/chakram/lib/chakram.js'),
    expect = chakram.expect;

const randomuserURL = "https://randomuser.me/api/";

describe("try random user API in different format", function () {
  it("should return JSON", function () {
    return chakram.get(randomuserURL).then(function(response){
      console.log(response.body);
      expect(response).to.have.status(200);
    });
  });

  it("should return YAML", function () {
    return chakram.get(randomuserURL + "?format=yaml").then(function(response){
      console.log(response.body);
      expect(response).to.have.status(200);
    });
  });

  it("should return XML", function () {
    return chakram.get(randomuserURL + "?format=xml").then(function(response){
      console.log(response.body);
      expect(response).to.have.status(200);
    });
  });

});
```

##### 多用户
`?results=5000`参数可以一次性返回多个用户数据。上限是5000。
```js
var chakram = require('./node_modules/chakram/lib/chakram.js'),
    expect = chakram.expect;

const randomuserURL = "https://randomuser.me/api/";

describe("try random user API", function () {
  it("should two prettyJSON", function () {
    return chakram.get(randomuserURL + "?format=pretty&results=2").then(function(response){
      console.log(response.body);
      expect(response).to.have.status(200);
    });
  });

});
```

##### 定制性别相关数据
`?gender=male`或`?gender=female`可以返回数据去接近于male还是female
```js
var chakram = require('./node_modules/chakram/lib/chakram.js'),
    expect = chakram.expect;

const randomuserURL = "https://randomuser.me/api/";

describe("try random user API", function () {

  it("should return male", function () {
    return chakram.get(randomuserURL + "?gender=male").then(function(response){
      console.log(response.body);
      expect(response).to.have.status(200);
    });
  });

});

```

##### 产生一样的数据

`?seed=danteyu`可以让每次执行产生一致的数据。Seeds可以为任意字符串。比如下面的代码重复执行会产生一致的结果。
```js
var chakram = require('./node_modules/chakram/lib/chakram.js'),
    expect = chakram.expect;

const randomuserURL = "https://randomuser.me/api/";

describe("try random user API", function () {

  it("should seed", function () {
    return chakram.get(randomuserURL + "?seed=danteyu").then(function(response){
      console.log(response.body);
      expect(response).to.have.status(200);
    });
  });

});
```

##### Locale
locale可以通过`?nat=us`。通过设置locale，可以返回更加真实的适用于不同国籍的数据。
```js
var chakram = require('./node_modules/chakram/lib/chakram.js'),
    expect = chakram.expect;

const randomuserURL = "https://randomuser.me/api/";

describe("try random user API", function () {

  it("should return CH", function () {
    return chakram.get(randomuserURL + "?nat=ch").then(function(response){
      console.log(response.body.results[0]);
      expect(response).to.have.status(200);
    });
  });
});

```

##### 刷选特定数据
如果只是想要返回特定的几个数据可以使用`?inc=gender,name,nat`来包含只需要返回的数据或是`?exc=login`来排除不想要的数据。
```js
var chakram = require('./node_modules/chakram/lib/chakram.js'),
    expect = chakram.expect;

const randomuserURL = "https://randomuser.me/api/";

describe("try random user API", function () {
  it("should return name and gender", function () {
    return chakram.get(randomuserURL + "?inc=name,gender").then(function(response){
      console.log(response.body);
      expect(response).to.have.status(200);
    });
  });

});
```

使用`?noinfo`可以只返回用户数据而没有seed,result和version等数据。
```js
var chakram = require('./node_modules/chakram/lib/chakram.js'),
    expect = chakram.expect;

const randomuserURL = "https://randomuser.me/api/";

describe("try random user API", function () {
  it("should return only user data", function () {
    return chakram.get(randomuserURL + "?noinfo").then(function(response){
      console.log(response.body.results);
      expect(response).to.have.status(200);
    });
  });

});

```
下面为输出
```
> randomuser@1.0.0 test /Users/diyu/workspace/chakram
> mocha demo.js

  try random user API
[ { gender: 'male',
    name: { title: 'mr', first: 'ryan', last: 'mackay' },
    location:
     { street: '6297 richmond ave',
       city: 'stratford',
       state: 'new brunswick',
       postcode: 77478 },
    email: 'ryan.mackay@example.com',
    login:
     { username: 'heavywolf629',
       password: 'method',
       salt: 'EVZMskM7',
       md5: '6756ee051abaccca0214c997204d8d67',
       sha1: '4c265c506210e365c9e1acadcdc2321fe899d8c4',
       sha256: '90dfbb565010f9524be4531ec8fc712aad3fca69658976c8dc7615a84afa86b9' },
    dob: '1979-08-06 17:27:58',
    registered: '2002-04-02 00:35:37',
    phone: '448-153-9250',
    cell: '979-394-3188',
    id: { name: '', value: null },
    picture:
     { large: 'https://randomuser.me/api/portraits/men/62.jpg',
       medium: 'https://randomuser.me/api/portraits/med/men/62.jpg',
       thumbnail: 'https://randomuser.me/api/portraits/thumb/men/62.jpg' },
    nat: 'CA' } ]
    ✓ should return only user data (990ms)

  1 passing (999ms)
```

#### Faker
类似`python`的`faker`提供了各类各样数据。Faker的使用非常简单，直接引用调用就行了。下面例子列举了部分常用数据类型。
```js
const faker = require('faker');

//address
console.log(faker.address.zipCode());
console.log(faker.address.city());
//commerce
console.log(faker.commerce.product());
console.log(faker.commerce.department());
//company
console.log(faker.company.companyName());
console.log(faker.company.bsBuzz());
//date
console.log(faker.date.future());
console.log(faker.date.month());
console.log(faker.date.weekday());
//finance
console.log(faker.finance.account());
console.log(faker.finance.transactionType());
//internet
console.log(faker.internet.email());
console.log(faker.internet.ip());
//lorem
console.log(faker.lorem.words());
console.log(faker.lorem.text());
//name
console.log(faker.name.findName());
console.log(faker.name.jobTitle());
//phone
console.log(faker.phone.phoneNumber());
```

`Faker.fake()`可以接受不同的API来组合字符串。
```js
const faker = require('faker');
{% raw %}
console.log(faker.fake("{{company.companyName}}, {{internet.email}} {{phone.phoneNumber}}"))
{% endraw %}
```
同样地，`faker`也支持多个locale。通过下面代码，可以进行设置。
```js
const faker = require('faker');
faker.locale = "zh_CN";
```

`faker`也支持`seed()`来产生一致的数据，这样的数据多用于单元测试等场景中。重复执行下面的代码会产生一致的结果。
```js
const faker = require('faker');
faker.seed(123);
console.log(faker.company.companyName());
```
`faker`也有[CLI](https://github.com/lestoni/faker-cli)和[JSON Schema](https://github.com/pateketrueke/json-schema-faker/)对应的实现。

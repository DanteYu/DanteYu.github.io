---
layout:     post
title:      测试设计思路总结
subtitle:   ""
date:       2017-03-27 12:00:00
author:     DanteYu
tags:
    - Test Design
---

#### 来自于局部探索式测试的思考思路
* Business value
* Input/output
* State Change
* Code Path
* User Data
* Execute Environment

#### 我们可以从下面分类来思考我们的测试用例设计

1. Structure - code path
- Test what is made of
2. Functions
- Test what it does
3. Business value
- Value added to customer
- Dependency
- Do what we don’t want to
- Impact others
4. Platform - Execute environment
- Test what is depends on
5. Data - Different type of data
- Valid & invalid
- Volume
- Test what is input and ourput
6. Interfaces
 - Test the ways we can interact with
        - With human
        - With program
7. State
- Test it’s different status
        - Persistence
        - Consistence
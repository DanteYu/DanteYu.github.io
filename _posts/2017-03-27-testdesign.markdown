---
layout:     post
title:      测试设计思路总结
subtitle:   ""
date:       2017-03-27 12:00:00
author:     DanteYu
tags:
    - test design
---

#### 来自于局部探索性测试的思考思路
* Business value
* Input/output
* State Change
* Code Path
* User Data
* Execute Environment


#### 我们可以从下面分类来思考我们的测试用例设计

1. Structure - code path
    1. Test what is made of
2. Functions
    1. Test what it does
3. Business value
    1. Value added to customer
    2. Dependency
    3. Do what we don’t want to
    4. Impact others
4. Platform - Execute environment
    1. Test what is depends on
5. Data - Different type of data
    1. Valid & invalid
    2. Volume
    3. Test what is input and ourput
6. Interfaces
    1. Test the ways we can interact with
        1. With human
        2. With program
7. State
    1. Test it’s different status
        1. Persistence
        2. Consistence
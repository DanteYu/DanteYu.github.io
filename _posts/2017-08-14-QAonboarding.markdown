---
layout:     post
title:      QA如何快速熟悉新项目
subtitle:   使用onboarding checklist来梳理项目情况
date:       2017-08-14 12:00:00
author:     DanteYu
tags:
    - QA
---

### 背景

在我现在的工作环境中，项目更换是一些做短期项目的同事经常遇到的事情；对于长期项目，由于各种原因(比如客户组织结构的变化)也会让不同角色的同事面临roll off和onboarding，通常来讲在一个项目做上1-2年已经是很长时间了。

### 目的

对于QA来说，当新上一个项目的时候需要尽快尽早熟悉项目环境和产品，对团队做出贡献。那如何做到这点呢？除了团队会提供相应的培训，QA们也应该有自己的方法来快速掌握项目上的知识。

我们可以使用onboarding checklist来从不同维度了解项目。这个checklist包含了QA应该了解的项目的各个方面，可以成为QA自己实践学习快速融入项目的参考，也可以成为团队培训新人的材料。

### QA onboarding checklist 内容

QA可以从下面几个维度着手来上手项目

#### 团队结构
* 公司背景
  * 行业背景
  * 部门背景
* 项目组背景
  * 项目组职责
  * 大项目组的成员架构
* 人员组成
  * 团队不同角色和其责任
  * 客户/服务对象
  * 依赖项目人员联系方式
  * 我的使命
  * 团队对我的期望
* 交流方式
  * 工具：邮件，skype
  * 加入相应的邮件组
* 团队工作方式
  * Way of Working
    * Escalation
    * Social contract

#### 项目情况
* 项目依赖服务
  * 上游/下游项目/团队
  * 第三方服务集成
  * 相互依赖团队协作方式
* 知识管理
  * 工具：Confluence
* 需求管理
  * 工具：JIRA
  * 故事墙
  * Backlog
* 风险管理
* 项目专用术语
* Release
  * 过往release情况
  * 当前release进展
* Roadmap

#### 产品知识
* 产品业务知识
* 产品特点
  * 接口
  * 数据
  * 功能
  * 平台
  * 操作
* 产品不同环境的访问地址
* 产品代码库
  * 代码托管服务地址
  * 开发环境搭建教程
* 产品专用术语
* 产品专家

#### 项目开发流程
* 敏捷实践
  * Agile ceremony
    * Standup
    * Retro
    * IPM
    * Kick off
    * Backlog grooming
    * Shoulder check
    * Elaboration
    * Estimation
    * Showcase
  * TDD
    * UT coverage
  * Pair
  * Code Review
  * Tech huddle
* 故事卡流程
* 常用工具
  * IDE
  * 自动填表单脚本
  * 定制shell

#### 项目开发技术
* 项目架构设计
* 项目前端技术
* 项目后端技术
* 项目数据库技术
  * 工具
  * 权限
* 项目构建技术
  * 工具： Gradle
* 项目持续集成
  * 工具： Jenkins
  * 流水线
  * 发布包管理库
* 项目版本控制
  * 工具： Git
* 项目部署技术
  * 蓝绿部署
* 项目发布流程
* 项目运维情况
  * 云服务 AWS
* 项目日志管理
  * 工具： Splunk
* 项目代码规范
  * checkstyle
  * unused import
* 项目开发流程规范
  * Git forking工作流
  * Frequent commit
  * Pull request
  * Broken build
* 项目线上问题处理流程
* 安全策略

#### 项目质量保证
* 质量标准
* 测试策略
  * 测试流程
  * 测试类型
  * 测试方法
    * 探索式测试
    * 自动化测试
      * 项目地址
      * 技术栈
      * 运行情况
    * 非功能性测试
  * 测试交付物
    * 测试报告
    * 质量报告
    * 用例管理
    * 缺陷管理
* 测试团队状况
  * 其他测试人员
  * 以往测试结果
  * 我需要具有哪些技术和能力
  * 团队还有哪些其他的技术和能力
  * 测试小组可能缺少哪些技术和能力
* 测试设备工具准备
  * postman
  * browserstack
* 质量保障体系

**要点**
1. 上面的checklist中，有些条目是有链接地址的，切记要把正确的地址链接到位
2. 上面的checklist中，有些工具是需要申请权限的，记住都要申请和developer相同的权限，比如代码库提交的权限
3. 上面的checklist中所列出的工具或技术只是参考

### 如何使用这个onboarding checklist

- **自己用**： 如果你是自己总结了一个onboarding checklist，可以自己walk through这个checklist，查漏补缺。对于checklist里面的某些方面，如果项目里还没有对应的内容，那恭喜你，这将会是你接下来需要思考的问题。
- **团队用**： 不同的软件项目都是不一样的，上面的checklist是一个“大而全”的列表，团队需要根据自己的项目情况对这个checklist进行定制化；此外，每次对onboarding checklist的使用都会收集到一些反馈(比如项目的复杂度增加)，我们需要更新一些条目，所以这个onboarding checklist也是需要被持续更新和改进的，保证和项目最新情况同步。团队可以根据checklist的内容，找团队不同成员给新人做培训。这也是新人的参考资料。

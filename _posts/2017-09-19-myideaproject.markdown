---
layout:     post
title:      我的乌托邦项目
subtitle:   
date:       2017-09-19 12:00:00
author:     DanteYu
tags:
    - PM
---
在最近几年的工作的项目都是离岸交付的敏捷项目，工作时间越长我越会思考我理想的项目是什么？我想在什么样的项目下工作来帮助个人的成功？不同的项目是否有着相似的特点？是否存在一个“标准化”的项目实践来指导我们把项目做好？

为了回答上面这些问题，我结合近几年的工作经验和项目经历，总结了下面的维度和实践。这些维度和实践都是可以落地，在我们新起项目的时候可以参考来创建项目标准，也可以在项目还处于交付中的时候参考来回顾改进。

##### Delivery Assurance
* 技术相关
  * 技术架构
    * 逻辑功能模块
    * 分层架构
    * 集成组件
    * 架构约束
  * 技术栈
    * 前端
    * 后端
    * Mobile
    * 开发支撑
    * 运维
  * 技术雷达
  * 技术演进路线图
  * 自动化测试
    * 自动化测试策略金字塔
  * 测试策略
  * 测试驱动开发 TDD
    * UT coverage
  * DevOps
    * 部署架构
      * 容器
      * 负载均衡
      * 反向代理
      * 防火墙
      * 存储系统
      * 集成系统
    * 自动化部署方案
      * 蓝绿部署
      * 金丝雀部署
    * 持续集成/持续部署
    * On-Demand Infrastructure
  * 安全
    * Threat modeling
    * 渗透测试
  * 性能
    * 并发访问
    * 响应速度
    * 资源利用率
  * 兼容性
    * 平台
    * 浏览器
  * 日志监控
* 业务需求相关
  * 商业目标
  * 产品愿景
    * 产品策略
    * 产品发展蓝图
  * 干系人管理
  * Inception
  * 范围管理
    * 明确交付范围。什么是必须的，什么是范围外的
  * 优先级管理
    * 确定MVP(most viable product)
  * 增量提交 Incremental Delivery
  * 在制品限制
  * 故事卡拆分编写
    * INVEST原则
    * 验收准则
* 敏捷项目相关
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
  * 故事卡流程  
  * Pair
  * Code Review
  * Tech huddle
  * 发布计划/Flight Plan
    * 可视化
  * RAID
    * Risk
    * Assumption
    * Issue
    * Dependency
  * Backlog Grooming
    * 保证backlog包含合适的item
      * 删除所有不相关的故事卡
      * 增加必要的故事卡来满足新挖掘的需求
      * 给没有点数的故事卡估点？
      * 纠正有变更的故事卡的估点？
      * 对于不适合进入下个迭代的故事卡进行拆卡
    * 所有故事卡都有相应的优先级
      * 重新评估有调整的故事卡的优先级
    * 优先级最高的故事卡 ready for delivery
  * Decision log
  * 迭代报告

##### 团队结构
* Leverage
  * 匹配项目类型的lead,senior,junior比例
* Onboarding & Rolloff process
  * 快速帮助新人上项目的流程和培训。比如申请工作机器和账号权限。
* Second-tier
  * 人才梯队. 培养team lead/tech lead的backup
  * No single point of failure. 避免单个成员具有不可替代性

##### 团队能力建设
  * 团队能力矩阵 Capability Matrix
    * 可视化 visualization
    * 跟踪 track
  * 能力建设计划 Capability Planning
    * 依据能力矩阵来做出能力建设计划
    * 可以考虑制定团队内某领域的业务专家或是技术专家
  * Newcomer Training
    * 帮助新人尽快上手项目，做出贡献
    * 可以多人协作完成
  * Personal Development
    * 个人的发展方向与项目契合
    * 个人能够在项目内发挥自己最大的潜力

##### 团队文化
  * 充满正能量
    * 不畏惧困难
    * 自信
    * 互相信任和帮助
  * 乐于分享
  * 持续改进
    * challenge the status quo
    * 敢于做出改变
    * safe to fail
  * Social Contract
    * social contract在敏捷团队中越来越流行，对于组员关系，团队表现和产品质量都有着显著的影响。Social Contract是一组团队达成共识的规则，包含了一些关于团队如何运转工作和沟通交流，给团队创建安全舒适的工作环境(safe to fail)，这是所有组员都要遵守的规则，无论角色职责的不同。
    * Example
      * 会议
        * 站会在9点半举行，不迟到
        * 在站会前就要把自己的卡更新了
        * 会议要有invitation和agenda，meeting minutes会在会议后共享
        * 收到agenda的时候，要回复会议organizer是否参加会议
        * 会议要有准备的参加
      * Way of Working
        * 团队不允许bullying
        * 对分配给自己的任务要有owner意识
          * 不要做假设
      * 交流
        * 要聆听和理解别人的观点
        * 问题的解决超出职责或能力范围的时候，要尽快主动寻求帮助
        * 面对面的交流优于电话交流，电话交流优于邮件交流
        * 没有silly question
        * 不要在别人说话的时候打断对方
        * 分享成功
  * Team Building

##### Onshore和offshore的合作与交流
  * 相互认可和奖励
  * 可视化团队成就
  * offshore team参与更多的项目关键活动
    * 需求分析
    * 架构设计
    * 任何decision making  
    * Inception
  * Mutual feedback
  * Rotation
    * Trust and relationship build
    * 能切身体会offshore/onshore同事的工作习惯和环境，换位思考，发现改进点

##### 持续改进
* 有效的回顾会议
* 针对feedback的action并且指派owner
* 持续探索和引入新技术
* Innovation

##### 知识管理 Knowledge Management
* 团队/项目知识库
  * 文档协作管理工具
    * 产品知识
    * 团队信息
    * 环境账户
    * 发布信息
    * 会议纪要
* 内部分享
  * 技术实践
  * 产品介绍
  * 交付成功
  * 创新

##### 对外影响力 Impact Index
* 对外分享 - showcase
  * 技术实践
  * 产品介绍
  * 交付成功
* 创新
  * 业务创新。能为合作伙伴节约成本和产品用户创建更大价值
  * 技术创新。
* 协助其他团队
  * 跨团队解决问题
  * 快速响应

##### 项目成功度量 Success Measurement
* 完成交付约定
  * 交付有价值的功能
  * 及时
  * 质量高。线上问题少
* 从需求到上线的响应时间
* 反馈
  * 客户
  * 产品用户
* 实时数据
  * 上线之后的交易量/转化率

##### 工作环境 Workspace
* Always on TV
  * 模拟在一个办公区间上班
  * 一个良好的urgent issue call out channel
* 硬件
  * 充足的会议室且会议室都有支持video的设备
  * 宽敞的个人工作区域
  * 个人配有状况良好的摄像头、耳麦、键盘、双屏等
  * 网络状态良好，能支持视频通话
* 安全
* 弹性上班
  * 不打卡
  * 如遇请假，可以请同事帮忙告知

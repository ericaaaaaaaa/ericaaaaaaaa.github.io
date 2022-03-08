---
layout: post
title: 现代软件工程
subtitle: 《构建之法：现代软件工程》读书笔记
categories: SoftwareEngineering
tags: book-report software-engineering
---

# 【读书笔记】构建之法：现代软件工程

# 第一章 概论

## 1.1 软件 = 程序 + 软件工程

一个复杂的软件不但要有合理的软件架构 (Software Architecture)、软件设计与实现 (software Design, Implementation and Debug)，还要有各种文件和数据来描述各个程序文件之间的依赖关系、编译参数、链接参数，等等。

## 1.2 软件工程是什么

**软件工程**是把系统的、有序的、可量化的方法应用到软件的开发、运营和维护上的过程。

软件工程包括：
* **软件需求分析**
* **软件设计**
* **软件构件**
* **软件测试**
* **软件维护**

### 1.2.1 软件的特殊性

1. 复杂性 (Complexity)
2. 不可见性 (Invisibility)
3. 易变性 (Changeability)
4. 服从性 (Conformity)
5. 非连续型 (Discontinuity)

### 1.2.2 软件工程与计算机科学的关系

工程：创造性的运用科学原理，设计和实现建筑、及其、装置或生产过程；或者是在实践中使用一个或多个上述实体；或者是在实现这些实体的过程。

### 1.2.3 软件工程的知识领域

### 1.2.4 软件工程的目标——构建“足够好”的软件

* 用户满意度
* 可靠性
* 软件流程的质量
* 可维护性

1. 研发出符合用户需求的软件说明
2. 通过一定的软件流程，在预计时间内发布“足够好”的软件说明
3. 通过数据和其他方式展现所开发的软件是可以维护和继续发展的说明

**软件的种类**
* ShrinkWrap （在包装盒子里面的软件）
* Web APP （基于网页的软件）
* Internal Software （企业或学校或某组织内部的软件）
* Games （游戏）
* Mobile Apps （手机应用）
* Operating Systems （操作系统）
* Tools （工具软件）

# 第二章 个人技术和流程

PSP (Personal Software Process) 个人软件开发流程

## 单元测试

### 2.1.1 用 VSTS 写单元测试

### 2.1.2 好的单元测试的标准

单元测试应该准确、快速地保证程序基本模块的正确性。

单元测试需要有程序的作者编写，因为代码作者最了解代码的目的、特点和实现的局限性。

单元测试应：
* 速度快
* 认为构造数据，而非用随机数（可能导致错误不可复现）
* 覆盖所有代码路径，包括错误处理路径
* 测试公开的和私有的函数 / 方法

100% 代码覆盖率不能保证 100% 程序正确性：
* 未处理问题，如内存未释放
* 代码效能问题
* 多线程环境中的同步问题
* 其他与外部条件相关的问题

单元测试的运行与维护
* 团队一般在每日构建之后运行单元测试
* 单元测试一般与产品代码一起保存和维护

### 2.1.3 回归测试

在单元测试的基础上，建立关于某一模块的回归测试 (Regression Test)

> Regress: return to a worse or less developed state.

一旦有关的测试用例通过，我们就得到了此模块的功能基准线 (Baseline)，一个模块的所有单元测试就是这个模块最初的 Baseline。

工程师们应该在新版本上运行所有已通过的测试用例，以验证有没有“退化”情况发生。

如果倒退是由于模块功能发生了正常变化引起的，那么要修改测试用例，以便和新功能保持一致。

针对一个 Bug Fix，也要做 Regression Test，目的是：
* 验证新代码的确改正了缺陷
* 同时验证新代码有没有破坏模块的现有功能，有没有 Regression

回归测试最好自动化，因为这样就可以对每一个构建你快速运行所有的回归测试。

在一个项目的最后稳定阶段，需要做全面的回归测试，确保所有已修复的 Bug 的确得到了修复，且没有复发。

除回归测试外，还有功能测试——从用户的角度检查功能的完成情况。

## 效能分析工具

* 抽样 (Sampling)
  * 当程序运行时，每隔一段时间记录程序运行在哪个函数内，得到程序运行时间的大致印象
  * 优点：无需改动程序，运行较快
  * 缺点：结果不精确，不能准确表示代码中的调用关系树 (Call Tree)
* 代码注入 (Instrumentation)
  * 将检测的代码注入到每个函数中
  * 优点：测量结果精确
  * 缺点：程序运行时间长

| 效能分析名词 | 含义 |
| --- | --- |
| 调用者 Caller | 函数 `Foo()` 调用了 `Bar()`，`Foo()` 就是调用者 |
| 被调用函数 Callee | 见上，`Bar()` 就是被调用函数 |
| 调用关系树 Call Tree | 从程序的 `Main()` 函数开始，调用者和被调用函数形成树形关系——调用树 |
| 消失时间 Elapsed Time | 从用户的角度看程序运行所花的时间。 |
| 应用程序事件 Application Time | 应用程序占用 CPU 的事件，不包括调用者使用时间 |
| 本函数时间 Exclusive Time | 所有在本函数花费的时间，不包括被调用者使用的时间 |
| 所有时间 Inclusive Time | 包含本函数和所有调用者使用的时间 |

## 个人开发流程

# 第三章 软件工程师的成长

## 个人能力的衡量与发展

软件开发的工作量与质量衡量标准

* 项目的大小
  * 一般用代码行数表示
  * 用功能点表示
* 项目耗时
* 项目质量
  * 代码缺陷与代码行数比
  * re-work 数量（不与最终质量成正比关系）
* 项目是否按时交付

## 问题的层次

* 低层次问题（变成自动操作）
* 中间层次问题（花脑力解决）
* 高层次问题（无暇顾及）

# 第四章 两人合作

## 4.1 代码规范

1. 代码风格规范
   主要是文字上的规定
2. 代码设计规范
   牵涉到程序设计、模块之间的关系、设计模式等通用原则

## 4.2 代码风格规范

代码风格的原则是：简明，易读，无二义性。

### 4.2.1 缩进

用 4 个空格

### 4.2.2 行宽

100 字符

### 4.2.3 括号

在复杂的条件表达式中，用括号清楚地表示逻辑优先级

### 4.2.4 断行与空白的 {} 行

```c
if (condition) 
{
    DoSomething();
}
else
{
    DoSomethingElse();
}
```

### 4.2.5 分行

* 不要把多条语句放在一行上
* 更严格地说，不要把多个变量定义在一行上

### 4.2.6 命名

### 4.2.7 下划线

下划线用来分隔变量名字中的作用域标注和变量的语义。

### 4.2.8 大小写

一个通用的做法是：
* 所有的类型 / 类 / 函数名都用 Pascal 形式
* 所有变量都用 Camel 形式。
* 类 / 类型 / 变量用名词或组合名词形式
* 函数用动词或动宾组合词表示

### 4.2.9 注释

注释（包括所有源代码）应该只用 ASCII 字符，不用中文或其他特殊字符。

## 4.3 代码设计规范

### 4.3.1 函数

### 4.3.2 goto

函数最好有单一的出口，为达到此目的，可以使用 goto

### 4.3.3 错误处理

### 4.3.4 如何处理 c++ 中的类

## 4.4 代码复审

* 自我复审
* 同伴复审
  * 不止一人
  * 找熟悉代码且有经验的人
* 团队复审

### 4.4.1 为什么要代码复审

* 发现代码问题
  * 编码错误
  * 风格问题
  * 逻辑错误
  * 算法错误
  * 潜在错误与回归性错误
* 发现可能的改进之处
* 从代码中学习

### 4.4.2 代码复审的步骤

* 代码必须通过编译
* 代码必须通过测试

复审结果
* 打回
* 有条件同意
* 放行

## 4.5 结对编程

### 结对编程的优点

1. 提供更好的设计质量和代码质量，两人合作解决问题的能力更强
2. 带来更多信心，高质量的产出带来满足感
3. 更有效的交流，相互学习和传递经验，分享知识，更好的应对人员流动

# 第五章 团队和流程

## 5.1 非团队和团队

## 5.2 软件团队的模式

## 5.3 开发流程

### 5.3.1 写了再改模式 (Code-and-Fix)

适用场景：
* “只用一次”的程序
* “看过了就扔”的原型
* 一些不实用的演示程序

### 5.3.2 瀑布模型 (Waterfall Model)

局限性：
* 各步骤之间相互分离
* 回溯修改困难，甚至不可能
* 最终产品知道最后才出现

### 5.3.2 瀑布模型的各种变形

* 生鱼片模型
* 大瀑布带小瀑布

### 5.3.4 Rational Unified Process (RUP)

重计划，重事先设计，重文档表达。

规程 (Discipline) 或工作流 (Workflow)
* 业务建模
  * 用精确的语言（通常是 UML）把用户的活动描述出来
* 需求
  * 分析并确认软件系统提供什么样的功能满足用户需求
* 分析和设计
  * 将需求转化成系统设计
* 实现
* 测试
* 部署
  * 将最终版本分发给最终用户
* 配置和变更管理
* 项目管理
  * 平衡各种可能产生冲突的目标，管理风险
* 环境
  * 向软件开发组织提供软件开发环境，包括过程和工具

阶段
* 初始阶段
  * 分析软件系统大概的构成，系统与外部的边界在哪里
* 细化阶段
  * 分析问题领域，建立健全的体系结构基础，编制项目计划，按优先级处理项目中的风险。
* 构造阶段（beta）
  * 开发出所有的功能集，有秩序地把功能集成为经过各种测试验证过的产品
* 交付阶段
  * 确保软件能满足最终用户的实际需求

### 5.3.5 老板驱动的流程 (Boss-Driven Process)

### 5.3.6 渐进交付的流程 (Evolutionary Delivery), MVP 和 MBP

[开发 $\rightarrow$ 发布 $\rightarrow$ 听取反馈 $\rightarrow$ 根据反馈做改进]

# 第六章 敏捷流程

## 6.1 敏捷的流程

### 6.1.1 敏捷开发原则

1. **尽早**并**持续**地交付有价值的软件以满足顾客需求
2. 敏捷流程欢迎**需求的变化**，并利用这种变化来提高用户的竞争优势
3. 经常发布可用的软件，发布间隔可以从几周到几个月，**能短则短**
4. **业务人员和开发人员**在项目开发过程中应该每天共同工作
5. 以**有进取心**的人为项目核心，充分信任他们
6. 无论团队内外，**面对面**的交流始终是最有效的沟通方式
7. **可用**的软件是衡量项目进展的主要指标
8. 敏捷流程应能保持可持续发展。领导、团队和用户应该按照目前的步调持续合作下去
9. 只有不断关注技术和设计，才能越来越敏捷
10. 保持**简明**——尽可能简化工作量
11. 只有能自我管理的软对才能创造优秀的架构、需求和设计
12. 时时总结如何提高团队效率，并付诸行动

### 6.1.2 敏捷流程概述

1. 找出产品需要做的事情——Product Back-log
2. 决定当前的冲刺 (Sprint) 需要解决的事情——Sprint Backlog
3. 冲刺 (Sprint)
   * 每日例会 (Scrum Meeting)
   * 燃尽图 (Burn Down Chart)
     * 实际剩余时间 (Remaining Hour)
     * 预估剩余时间 (Projected Remaining Hour)
     * 实际花费时间 (Completed Hour)
   * 看版图 (Kanban)
4. 得到软件的一个增量版本，发布给用户。

## 6.2 敏捷流程的问题和解法

## 6.3 敏捷的团队

## 6.4 敏捷总结

## 6.5 敏捷的故事

# 第七章 MSF

## 7.1 MSF 简史

## 7.2 MSF 基本原则

1. 推动信息共享与沟通 (Foster open communications)
2. 为共同的远景而工作 (Work toward a shared vision)
3. 充分授权和信任 (Empower team members)
4. 各司其职，对项目共同负责 (Establish clear accountability and shared responsibility)
5. 交付增量的价值 (Deliver incremental value)
6. 保持敏捷，预期和适应变化 (Stay agile, expect and adapt change)
7. 投资质量 (Invest in quality)
8. 学习所有经验 (Learn from all experiences)
9. 与顾客合作 (Partner with internal and external customers)

# 第八章 需求分析

## 8.1 软件需求

1. 获取和引导需求 (Elicitation)
   软件企业 = 软件 + 商业模式
2. 分析和定义需求 (Analysis & Specification)
3. 验证需求 (Validation)
4. 在软件产品的生命周期中管理需求 (Management)
   * 功能性要求
   * 产品开发过程的需求
   * 非功能性需求（服务质量需求）
   * 综合需求

## 8.2 软件产品的利益相关者

* 用户（最终用户）
* 顾客（客户）
* 软件工程师

## 8.3 获取用户需求——用户调查

1. 焦点小组 (Focus Group)
   找到目标用户的代表，加上项目的利益相关者
2. 深入面谈 (In-depth Interview)
3. 卡片分类 (Card Sorting)
4. 用户调查问卷 (User Survey)
   常见错误
   * 问题定义不准确
   * 使用含糊不清的形容词
   * 让用户花额外的努力来回答问题
   * 问题带有有引导性的倾向
   * 问题涉及用户隐私
   问题形式
   * 全开放式问题
   * 二项选择题
   * 多项选择题
   * 顺位选择题（优先级）
5. 用户日志研究 (User Diary Study)
6. 人类学调查 (Ethnographic Study)
7. 眼动跟踪研究
8. 快速原型调研
9. A/B 测试 (A/B Testing)

## 8.4 竞争性需求分析的框架

NABCD 模型

1. N (Need，需求)
2. A (Approach，做法)
3. B (Benefit，好处)
4. C (Competitors，竞争)
5. D (Delivery，推广)

## 8.5 功能的定位和优先级

* 功能和需求分类
* 杀手功能
* 外围功能
* 必要需求
* 辅助需求

做法的分类
* 维持
* 抵消
* 优化
* 差异化
* 不做

## 8.6 计划和估计

整个软件项目的时间估计：
* 自底向上
* 回溯

在敏捷开发中不需要精确的估计，只需要粗粒度的估计，然后进入实现阶段并不断复盘即可。

## 8.7 分而治之 (Work Breakdown Structure, WBS)

1. 保证所有子结点覆盖了全部父节点包含的内容
2. 保证各个子结点不要相互覆盖
3. 叶子结点要保证足够小，能在一个里程碑中完成
4. 从结果 (Outcome) 出发构建 WBS，而不是从团队的活动 (Action) 出发

# 第九章 项目经理

## 9.1 PM 是啥

PM
* Product Manager: 产品经理
* Program Manager: 项目经理

## 9.2 微软 PM 的来历

# 第十章 典型用户和场景

# 第十一章 软件设计与实现

# 第十二章 用户体验

# 第十三章 软件测试

## 13.1 基本名词解释及分类

* Bug: 软件的缺陷
* Test Case: 测试用例
* Test Suite: 测试用例集

### 13.1.1 按测试设计的方法分类

* 黑箱 (Black Box)
* 白箱 (White Box)

### 13.1.2 按测试的目的分类

### 13.1.3 按测试的时机和作用分类

## 13.2 各种测试方法

### 13.2.1 单元测试 (Unit Test)

### 13.2.2 代码覆盖率测试 (Code Coverage Analysis)

### 13.2.3 构建验证测试 (Build Verification Test, BVT)

### 13.2.4 验收测试 (Acceptance Test)

### 13.2.5 “探索式”的测试 (Ad hoc Test)

### 13.2.6 回归测试 (Regression Test)

### 13.2.7 场景 / 集成 / 系统测试 (Scenario / Integration / System Test)

### 13.2.8 伙伴测试 (Buddy Test)

### 13.2.9 效能测试 (Performance Test)

### 13.2.10 压力测试 (Stress Test)

### 13.2.11 内部 / 外部公开测试 (Alpha / Beta Test)

### 13.2.12 易用性测试 (Usability Test)

### 13.2.13 “小强”大扫荡 (Bug Bash)

# 第十四章 质量保障

## 14.1 软件的质量

## 14.1.1 程序的质量

* 准确度 (Precision)
* 覆盖率 (Recall)
* 速度
* 吞吐量
* 用户数量
* ……

## 14.1.2 软件工程的质量

* 软件开发过程的可见性 (Visibility)
* 软件开发过程的风险控制 (Risk Management)
* 软件内部模块，项目中间阶段的交付质量，项目管理工具的因素
* 软件开发成本的控制 (Cost Control)
* 内部质量指标的完成情况 (Internal Benchmarks)

### 14.1.3 软件工程的质量

CMMI (Capacity Maturity Model Integrated，能力成熟度模型集成)

# 第十五章 稳定和发布阶段

# 第十六章 IT 行业的创新

# 第十七章 人、绩效和职业道德
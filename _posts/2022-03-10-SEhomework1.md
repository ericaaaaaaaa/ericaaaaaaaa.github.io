---
layout: post
title: 软件工程作业一——个人阅读和调研
subtitle: 2022 北航敏捷软件工程作业
categories: SoftwareEngineering
tags: homework software-engineering
---

# 个人阅读作业——阅读和调研

| **项目** | **内容** |
| --- | --- |
| 这个作业属于哪个课程 | [2022 年北航敏捷软件工程](https://bbs.csdn.net/forums/BH-SE) |
| 这个作业要求再哪里 | [个人阅读作业-阅读和调研](https://bbs.csdn.net/topics/605073900) |
| 我在这个课程的目标是 | 学习软件工程相关知识，提高自己的代码能力与团队协作能力。 |
| 这个作业在哪个**具体方面**帮助我实现目标 | 阅读《构建之法》帮助我初探软件工程，了解相关术语和最佳实践，提出问题环节锻炼我独立思考能力和批判性思维，调研工具链帮助我更好的完成之后的学习任务。 |

## 提问的智慧

[阅读笔记](https://ericaaaaaaaa.github.io/others/2022/03/05/HowToAskQuestion.html)

## 阅读提问

通过阅读邹欣老师的《构建之法》[$^{[1]}$](#参考文献)，我产生了如下疑惑：

### 关于代码

#### 问题 1：goto 的使用

书中 4.3.2 代码设计规范->goto 一节中，作者认为：

> 函数最好有单一的出口，为了达到这一目的，可以使用 `goto`。只要有助于程序逻辑的清晰体现，什么方法都可以使用，包括 `goto`。

但这有悖于以往的学者观点与企业实践，更具体的来说：

在上世纪学者就针对 `goto` 语句存在的合理性提出过质疑，著名的计算机科学家 Dijkstra 在 1968 年发表了一篇名为 Go to statement considered harmful[$^{[2]}$](#参考文献) 的论文，其中，他认为由于 `goto` 语句会破坏程序的逻辑完整性，加大程序验证与分析的难度，`goto` 指令应在高级程序设计语言中被废止。

很多企业的设计规范中也提到了对于 `goto` 语句的限制，如：华为 C/C++ 编程规范[$^{[3]}$](#参考文献)中就明确指出：

> 建议6.3 不要滥用 `goto` 语句。
> 说明：`goto` 语句会破坏程序的结构性，所以除非确实需要，最好不使用goto语句。

我认为在高级语言程序中应避免使用 `goto` 语句。实际上，很多如 Python, Java 等语言已经在其基本语法中取消了 `goto`。诚然，一些优秀的程序员可以利用 `goto` 写出逻辑清晰的程序，但我认为这些可以被其他条件语句，如 `if-else`, `switch-case` 等代替，且由于后者本身就具有良好的逻辑结构，因此并不会极大的降低程序的清晰程度。此外，不在高级语言程序中使用 `goto` 语句也能避免一些水平较低的程序员写出难以维护和理解的代码。

### 关于测试

#### 问题 2：单元测试的编写时间 (TDD / TLD)

在 2.1 节中的问答部分中，作者提到

> 最好是在设计的时候就写好单元测试，这样单元测试就能体现 API 的语义，如果没有单元测试，语义的准确性就不能得到保障，以后会产生歧义。

在 11.4 从 Spec 到实现中，作者列出了开发人员拿到设计文档后的工作步骤，其中，“创建或更新”单元测试被安排在了“写好代码”之后。

经查阅资料后得知，关于单元测试的具体编写时间（即放在编写代码前还是后）依然是存在争议的话题，对于该问题的两种答案也可以归为 TDD(Test Driven Development) 和 TLD(Test Last Development) 两个阵营。

两派的观点大概可以由下表总结：

| 观点 | 先写单元测试，后写代码 (TDD) | 先写代码，后写单元测试 (TLD) |
| --- | --- | --- |
| 优点 | 有助于在写代码前理解 API 语义，有助于写出架构优秀且简介的代码，避免重构 | 节省时间，对代码的针对性强 |
| 缺点 | 耗费时间长，在写码时可能会对 Test Case 反复修改，工期紧张时会压缩写代码的时间，甚至无法按时交付 | 难以保证代码的正确性，可能导致重构 |

此外，也有学者认为编写单元测试的时间与程序的质量和编写效率并不相关，Hussan Munir 等就在在一篇 2014 年发表于 ACM 上的文章[$^{[4]}$](#参考文献)中结合调查研究阐释了这一观点，原文如下：

> Experiment results indicate that values found related to number of acceptance test cases passed, McCabe's Cyclomatic complexity, branch coverage, number of lines of code per person hours, number of user stories implemented per person hours are statistically insignificant.

针对上述调研，我的问题是，该如何选择编写单元测试的时间？

#### 问题 3：单元测试的编写人

在 2.1 节单元测试中，作者提到单元测试可以有效解决“程序员对模块功能的误解、疏忽或不了解模块的变化”，可以使得“模块功能定义尽量明确，模块内部的改变不会影响其他模块，且模块的质量能得到稳定的、量化的保证。”

在后文中，作者提出“单元测试必须由最熟悉代码的人（程序的作者）来写”。

但我对单元测试的编写人有所疑问，若程序的作者本身对模块功能存在误解，那么程序作者本身写出的单元测试也难免会存在一定的偏差；又或者作者本身在某一功能的理解上存在疏忽，那其写出的单元测试也难以避免会漏掉某些检查点，因此，我认为自己编写的单元测试可能无法有效的测出程序内存在的 bug，达到单元测试的目的。

除此之外，由于在软件工程中有专门进行测试的人员，且作者又在 13.2.8 节中提到“伙伴测试”的概念，我认为，让专业的测试人员，尤其是对程序功能有着更深入理解的“伙伴”进行单元测试的编写或许可以达到更好的效果。

### 关于用户体验

#### 问题 4：定义“典型用户”是否能改善用户体验

《构建之法》第 10 章中详细的描述了典型用户和场景的相关内容，在 10.1.2 典型用户的价值一节中，作者提到

> 搞一个“典型用户”会强迫我们在考虑问题时从用户的角度出发。

而在 8.3 节“获取用户需求——用户调查”中，作者列举了一系列调研用户需求的方式，如“人类学调查”、“A/B 测试”等，我认为需要这些复杂的调查的根本原因在于工程团队可能不够了解用户的需求，或是如作者所说：“对这些需求不够敏感”。

典型用户是由工程团队所定义出的虚拟用户，含有一定的主观因素，由于软件开发人员本身可能不够了解用户需求，因此其定义的典型用户也可能不具有代表性，难以避免含有设计团队的刻板印象，或者难以真实的代表现实用户群体。如上所述定义出的典型用户由于和真实用户之间存在差异，因此对照其设计出的软件也可能无法改善用户体验。

一些学者，如微软的 Christopher N. Chapman 也在论文[$^{[5]}$](#参考文献)中提到过类似的观点，

> We argue that there are significant methodological and practical difficulties for personas. It is difficult to determine how many, if any, users are represented by a persona, and thus is difficult to know whether a persona is relevant for intended users. Personas cannot be adequately verified or falsified and therefore have no demonstrable validity. We believe personas are likely to lead to political conflicts and to undermine the ability for researchers to resolve questions with data.

因此，我对书中提到的定义“典型用户”的实用性持怀疑态度，同时，我也想知道如何保证典型用户的代表性（是否能结合一些统计学方法），尽量减少主观因素和偏见。

### 关于编程方式

#### 问题 5：“结对编程”的效率问题

在 4.5 节中，作者介绍了结对编程的形式：

> 在结对编程模式下，一对程序员肩并肩、平等地、互补地进行开发工作。他们并排坐在一台电脑前，面对同一个显示器，使用同一个键盘、同一个鼠标一起工作。他们一起分析，一起设计，一起写测试用例，一起编码，一起做单元测试，一起做集成测试，一起写文档等。

作者认为，“如果运用得当，结对编程可以取得更高的投入产出比”。

我并没有过结对编程的体验，但根据描述，我认为，虽然结对编程可能会减少 bug 的出现，但由于同一时间只有一个人在编程，结对编程的效率可能依然会低于两人分别编程；且就我个人经验而言，当他人看着我工作时，我的注意力会极大的减弱。为此，我也寻找到了文献支持，如一篇 2008 年发表在 ACM 上的文章[$^{[6]}$](#参考文献)就大规模地调查了微软员工的结对编程效果，调查显示，65.4% 的人认为结对编程会产出更加高质量的代码，但谈及效率问题，只有 25.4% 的人认为结对编程不存在低效的问题。

由于上述原因，我对于结对编程的效率存疑，也对其在行业内的真实应用情况感到好奇。

## 调研源代码版本管理软件

我调研了目前较为流行的源代码版本管理软件 Git 的项目管理工具，结果如下：

### GitHub
#### 简介

Github 是通过 Git 进行版本控制的软件源代码托管服务平台，是目前世界上最大的代码存放网站和开源社区。在 2018 年 GitHub 被微软收购。

#### 功能

Github 提供付费账户和免费账户，用户可以建立公开或私有的代码仓库。除了基本的版本控制功能外，GitHub 还提供了一系列类似社交网络的功能，如 Star、Follow、Comment 等。此外，Github 还提供了网页寄存服务 GitHub Pages，可以用于存放如博客、项目文档等静态网页。

### GitLab

#### 简介

GitLab 是由 GitLab Inc. 开发的基于 Git 的版本控制工具。由于 GitLab 有着较为完善的管理界面和权限控制功能，保密性强，因此其面向对象主要是企业，一般用于在企业或学校等内部网络中搭建 Git 私服。

#### 功能

GitLab 支持 wiki，CI/CD，在线编辑等功能。其中，GitLab Runner 可以执行任务 (Job)，并将执行结果传输回 Gitlab。

### Bitbucket

#### 简介

Bitbucket 是 Atlassian 公司提供的，基于 web 的版本管理服务，支持 Mercurial 与 Git（2011 年开始支持 Git）。和 GitHub 一样，Bitbucket 同时提供免费与付费方案，其中，免费账号的权力收到一定的限制。其使用人数远不及 Github。

#### 功能

Bitbucket 提供私有仓库， CI/CD，spoon 等服务，也可以托管简单的 html 界面。

### Gitee

#### 简介

Gitee，又称**码云**，是 OSCHINA.NET 推出的代码托管平台，支持 Git 和 SVN，提供中国本土化的代码托管服务，目前已有超过 800 万的开发者选择 Gitee。

#### 功能

Gitee 支持个人、企业 / 机构的代码管理与高校教学管理。提供免费静态网页托管 Gitee Pages，代码质量分析，在线 IDE 以及轻量级 Pull Request 等功能。除此之外，还有具有中国特色的钉钉与微信集成功能。

## 调研持续集成 / 部署工具

### 调研

CI/CD 的核心概念是持续集成、持续交付和持续部署。([参考博客](https://www.mindtheproduct.com/what-the-hell-are-ci-cd-and-devops-a-cheatsheet-for-the-rest-of-us/))

#### 持续集成 (Continuous Integration)

在传统项目中，构建、测试和集成通常频率较低（以周或月为单位，在所有成员完成工作时进行），持续部署可以在开发人员提交代码后立刻进行构建、测试，并根据测试结果，确定是否与原代码集成。

![](https://3lsqjy1sj7i027fcn749gutj-wpengine.netdna-ssl.com/wp-content/uploads/2015/12/409-images-for-snap-blog-postedit_image1.png)

#### 持续交付 (Continuous Delivery)

持续交付建立在持续集成的基础之上，可以将集成后的代码部署在更加贴近生产环境的环境中。

![](https://pic1.zhimg.com/80/db7198e3c39e4656e18efcb4bd1b20b1_720w.jpg?source=1940ef5c)

#### 持续部署 (Continuous Deployment)

持续部署建立在持续交付的基础上，将部署到生产环境的过程自动化。

![](https://3lsqjy1sj7i027fcn749gutj-wpengine.netdna-ssl.com/wp-content/uploads/2015/12/409-images-for-snap-blog-postedit_image3-auto.png)

### 实践

#### GitHub Action

利用 Jekyll 建立个人博客，并用 GitHub Action 在 push 时自动构建并部署到[个人主页](https://ericaaaaaaaa.github.io/)中。

其中，操作截图如下：

![image](https://img2022.cnblogs.com/blog/2324053/202203/2324053-20220310085324085-488383704.png)

部署结果（个人博客）如下：

![image](https://img2022.cnblogs.com/blog/2324053/202203/2324053-20220310085544176-1254980176.png)

> [代码仓库](https://github.com/ericaaaaaaaa/ericaaaaaaaa.github.io)
> [效果展示](https://ericaaaaaaaa.github.io/)

#### GitHub CI

利用 GitHub CI 进行简单的编译及单元测试。

编写 .github/workfolws/***.yml，文件内容如下：

```yml
# This workflow will install Python dependencies, run tests and lint with a single version of Python
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-python-with-github-actions

name: Python application # 名称

on:
  push: # 在 push 时触发操作
    branches: [ main ] # 分支名称
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest # 运行环境

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.10 # 构建 Python 版本
      uses: actions/setup-python@v2
      with:
        python-version: "3.10"
    - name: Install dependencies # 安装依赖包
      run: |
        python -m pip install --upgrade pip
        pip install flake8 pytest
        if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
    - name: Lint with flake8
      run: |
        # stop the build if there are Python syntax errors or undefined names
        flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
        # exit-zero treats all errors as warnings. The GitHub editor is 127 chars wide
        flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics
    - name: Test with pytest # 用 Pytest 进行测试
      run: |
        pytest
```

运行截图如下：

当单元测试存在问题时，会报错并停止运行，如下：

![image](https://img2022.cnblogs.com/blog/2324053/202203/2324053-20220310090211139-1041919025.png)

当单元测试顺利通过时，结果如下：

![image](https://img2022.cnblogs.com/blog/2324053/202203/2324053-20220310090355338-810533114.png)

> [代码仓库](https://github.com/ericaaaaaaaa/travis_test)
> [效果展示](https://github.com/ericaaaaaaaa/travis_test/actions/runs/1960446114)

#### CircleCI

利用 CircleCI 对数据库大作业前端（利用 Quasar 框架编写）进行构建和部署。（连接 GitHub 仓库，再利用 Surge 部署）

首先，先对 CircleCI 进行配置（链接 GitHub，设置环境变量等，具体可参考[博客](https://blog.csdn.net/zy1281539626/article/details/114832468?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164680824916780357260474%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164680824916780357260474&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-3-114832468.pc_search_result_control_group&utm_term=circleci&spm=1018.2226.3001.4187)）

编写 .circleci/config.yml，如下：

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:12.22.1 # 环境 Node 12.22.1
    branches:
      only:
        - circleci-project-setup # 分支
    steps:
      - checkout
      - run: echo "A first hello" # 打印内容
      - run: npm -v
      - run: npm install && npm install --only=dev
      - run:
          name: Install
          command: npm install
      - run:
          name: InstallQuasar # 安装 Quasar
          command: sudo npm install -g @quasar/cli
      - run:
          name: InstallSurge # 安装 Surge
          command: sudo npm install -g surge
      - run:
          name: Build # 利用 Quasar 对前端进行构建，并将结果写入 ./dist/spa 文件中
          command: quasar build 
      - run:
          name: ListDir # 查看当前目录内容
          command: dir
      - run:
          name: Run Deploy to Github pages # 利用 Surge 部署前端页面
          command: surge --project ./dist/spa --domain BUAA_Database_Final_Project.surge.sh
```

运行结果如下：

![image](https://img2022.cnblogs.com/blog/2324053/202203/2324053-20220310090921680-868406931.png)

详细结果如下：

![image](https://img2022.cnblogs.com/blog/2324053/202203/2324053-20220310091757556-1418404161.png)

部署展示：

![image](https://img2022.cnblogs.com/blog/2324053/202203/2324053-20220310091858571-1211313682.png)

> [代码仓库](https://github.com/ericaaaaaaaa/BUAA_Database_Final_Project/tree/circleci-project-setup)
> [效果展示](https://buaa_database_final_project.surge.sh/) （由于用 Surge 进行部署，因此项目存在一定的有效期，且由于只部署了前端，因此无法进行有效的内容交互）

### 总结

通过对于 CI/CD 的调研和使用，我了解了不同工具的优缺点，总结如下：

| 名称 | 功能 / 特点 | 优点 | 缺点 |
| --- | --- | --- | --- |
| GitHub Action | 内置于 GitHub 中，事件触发(pill, push, ...)，有免费和收费模式 | 有良好的版本控制功能，语法比 GitHub CI 相对更容易，有一些 GitHub 特有的功能（建立 GitHub Pages，管理 issue 等），官方市场上提供了很多有借鉴性的 Action 样例 | 配置文件与代码在一起，相对不隔离 |
| GitHub CI | 内置于 GitLab 中，版本管理容易 | 有良好的版本控制功能 | 配置文件与代码在一起，相对不隔离 |
| GitLab CI/CD | 内置于 GitLab 中，版本管理容易 | 有良好的安全和隐私政策，与 GitLab 融合，版本管理功能较为完善，有良好的 Docker 集成 | 多为企业和内部提供服务，需要支付一定的费用，配置文件与代码在一起，相对不隔离 |
| Circle CI | 基于云，不需要专用的服务器，提供免费与付费服务 | 可以管理 GitHub 上的代码，轻量级 | 免费版本功能有限，配置文件与代码在一起，相对不隔离 |
| Travis CI | 基于云，不需要专用的服务器 | 可以管理 GitHub 上的代码，比 Circle CI 支持更多语言，可以选择更多运行环境 | 比 Circle CI 价格更高，配置文件与代码在一起，相对不隔离 |
| Jenkins | 基于 Java 的开源独立程序，提供 1000+ 个插件来支持构建、部署、自动化、分布式等多种功能 | 免费，易于安装和配置，社区环境友好，采用插件提供了高度可定制性 | 服务需要安装，版本管理功能较弱，不允许完全控制分支，插件集成复杂，缺少对整个流程的跟踪分析，需要定期对插件进行更新 |

### 使用心得

在使用过程中，由于每种工具的语法与配置方式均存在或多或少的不同（即使 GitHub Action 与 GitHub CI 之间也存在语法差异），因此入手时都需要一定的时间。且根据需要部署的代码内容不同，对于环境的选择和相关说明文档的丰富程度也有较大的差异（较为流行的如 Python, Node.js 等说明文档较为完善，而前文中数据库前端所使用的 Quasar 框架相关的指导就很少）。

但配置完成后，对于代码的运行、测试和部署都变得非常简单。因此在以后的编码过程中，我也希望能够多使用 CI/CD 功能，提高自己的代码质量，也督促自己在编写时候多构建与测试。

## 参考文献

[1] 邹欣，构建之法（第三版），人民邮电出版社，2017-06

[2] E. Dijkstra. 1979. Go to statement considered harmful. Classics in software engineering. Yourdon Press, USA, 27–33.

[3] 华为技术有限公司 C语言编程规范，华为技术有限公司，2011

[4] Hussan Munir, Krzysztof Wnuk, Kai Petersen, and Misagh Moayyed. 2014. An experimental evaluation of test driven development vs. test-last development with industry professionals. In Proceedings of the 18th International Conference on Evaluation and Assessment in Software Engineering (EASE '14). Association for Computing Machinery, New York, NY, USA, Article 50, 1–10. DOI:https://doi.org/10.1145/2601248.2601267

[5] Chapman, CN; Milham, R (October 2006), "The personas' new clothes", Human Factors and Ergonomics Society (HFES) 2006 (PDF), San Francisco, CA

[6] Andrew Begel and Nachiappan Nagappan. 2008. Pair programming: what's in it for me? In Proceedings of the Second ACM-IEEE international symposium on Empirical software engineering and measurement (ESEM '08). Association for Computing Machinery, New York, NY, USA, 120–128. DOI:https://doi.org/10.1145/1414004.1414026
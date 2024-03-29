---
layout: post
title: 软件工程作业二——软件案例分析
subtitle: 2022 北航敏捷软件工程作业
categories: SoftwareEngineering
tags: homework software-engineering
---

# 个人作业-软件案例分析

| **项目** | **内容** |
| --- | --- |
| 这个作业属于哪个课程 | [2022 年北航敏捷软件工程](https://bbs.csdn.net/forums/BH-SE) |
| 这个作业要求再哪里 | [个人作业-软件案例分析](https://bbs.csdn.net/topics/605231753) |
| 我在这个课程的目标是 | 学习软件工程相关知识，提高自己的代码能力与团队协作能力。 |
| 这个作业在哪个**具体方面**帮助我实现目标 | 他山之石，可以攻玉。通过调研某一特定领域的软件，对自己接下来的软件开发设计起到启发和警示的作用。 |

## 第一部分 调研，评测

调研、试用了目前较为流行的代码仓库管理系统，以下是我的使用体验及发现的 bug：

### 功能体验

#### [GitLink](https://www.gitlink.org.cn/)

GitLink 是笔者因为此次作业第一次听说的平台，该平台的定位是“新一代开源创新服务平台”，目前用户量还较为有限，且还处于发展与完善阶段。其官方仓库不断在更新，且不断有新的 Issue 被提出。有趣的是，GitLink 的开发成员布置任务也会在 Issue 中进行，可以看到一个开发人员发 Issue 指明让另一位完善 / 修复某功能，或者成员之间将一个任务不断向下指派的场景。

![image](/assets/images/post/SE/gitlink.png)

除了基本的代码管理功能以外，GitLink 还有 wiki，里程碑，DevOps 等功能。由于是中国人开发的平台，因此也有独具特色的分享到微信的悬浮按键。

![image](/assets/images/post/SE/gitlink_wiki.png)

#### [GitCode](https://codechina.csdn.net/)

GitCode 是 CSDN 为开发者提供的开源项目创新服务平台。有开源仓库所具备的基本功能，如代码管理，分支管理，私人/公共仓库，流程图，与 wiki 等功能。

![image](/assets/images/post/SE/gitcode1.png)

除此之外，另我略感以外的是，GitCode 还支持 CI/CD （流水线）和部署静态网页的服务。虽然目前支持的模板还比较有限，但可以看到 GitCode 是希望努力成长为一个全面的代码仓库管理平台的。

![image](/assets/images/post/SE/gitcode2.png)

目前 GitCode 正处于快速成长与完善阶段，其运营仓库平均每天有 0.5 个 commits，且在其运营仓库和帮助文档中，也不断有新的 Issue 被提出。

和 GitHub 一样，GitCode 中也可以对项目进行标星 (Star) 操作，也可以关注 (Follow) 其他用户，跟踪其动态等。在 GitCode 中，最多的标星数目是 1037 个，远少于目前主流的 GitHub。

具有中国特色的一点是，GitCode 右侧的悬浮按钮之一是其微信企业号的二维码，点击可以进入交流群。

![image](/assets/images/post/SE/gitcode3.png)

#### [GitHub](https://github.com/)

GitHub 是全球最大的代码存放网站和开源社区，也是笔者日常会使用的代码浏览和管理平台。

![image](/assets/images/post/SE/github1.png)

除了正常的版本管理相关功能之外，GitHub 还提供了 CI/CD, Actions, GitHub Pages 等功能。它的 comment 和 star 等功能增强了 GitHub 的社交属性。此外，网站上甚至还有 Trending repositories / developers 的窗口，可以帮助用户了解领域热门的代码和开发者。

#### [Gitee](https://gitee.com/)

Gitee，又称码云，是开源中国推出的代码托管与协作平台。

![image](/assets/images/post/SE/gitee1.png)

相比于本文中调研的其他平台，Gitee 的面向群体更偏向于企业和团体，它的定位也是“企业级” DevOps 研发管理平台，在使用过程中也会不断邀请你开通企业版。并且在页面中也经常出现“特惠”、“免费”等字样，虽然看似十分优惠，却让笔者这类个人用户望而却步。

![image](/assets/images/post/SE/gitee2.png)

除此之外，Gitee 也具有中国特色，支持微信、钉钉集成等服务。

### Bug

在实际试用中，发现了部分平台的功能新 bug，具体如下：

#### 测试环境

Windows 10

Google Chrome Version 99.0.4844.51

#### 评估标准

| 星级 | 描述 |
| --- | --- |
| :star: | 有轻微影响 |
| :star::star: | 有一定影响 |
| :star::star::star: | 影响较大 |
| :star::star::star::star: | 影响严重 |
| :star::star::star::star::star: | 影响恶劣 |

#### GitLink--“论坛交流”板块“登录”跳转错误

##### bug 描述

未登录情况下，在“论坛交流”板块添加文章评论，或点击右上角“写点什么”时，会跳出登录窗口，点击该窗口中的“找回密码”和“注册”会分别跳转至 trustie（“确实”）平台的“找回密码”和“登录”界面。

错误的登录窗口（其“找回密码”和“注册”会分别跳转至 trustie（“确实”）平台的对应界面）

![image](/assets/images/post/SE/gitlink3.png)

正常的登录窗口

![image](/assets/images/post/SE/gitlink4.png)

##### bug 展示

![image](/assets/images/post/SE/gitlink5.gif)

##### bug 严重性

###### 系统功能

:star::star::star:

登录和注册是用户的主要需求之一（因为登录后才可以执行创建仓库、提交 Commit, Issue，添加评论等操作），但该 bug 会导致此功能无法正常使用，因此较为严重。

###### 安全性

:star::star::star::star:

若 trustie 发现了 GitLink 的这一漏洞，则可采集到原本属于 GitLink 用户的信息（账号，邮箱，密码等），从而造成 GitLink 用户信息存在泄露的风险（即使 trustie 无意获得用户信息，也可能因为其本身的安全漏洞使得其他人获得用户信息），因此我认为该 bug 的在安全性上较为严重。

###### 用户体验

:star::star::star:

若用户在“论坛交流”板块进行注册操作，则无法正常注册 GitLink 账户，而会意外的注册 trustie 平台账户（**两平台账户不通用**），再返回 GitLink 时会发现无法正常登录，且无法找到刚注册的账号，这会给用户的使用造成极大的不便，且可能会让用户认为自己的个人信息（账号密码）存在泄露的风险。

但由于从“论坛交流”板块注册的用户可能相对较少，因此对大部分用户而言，在 GitLink 上的登录注册功能是可以正常使用的。

##### 对 bug 的预期及改进建议

希望 GitLink 平台可以尽快修复 bug，更改“论坛交流”板块的登录注册的跳转动作，从而使得用户不会在使用途中“被”切换到其它平台，并面临个人信息泄露的风险。

##### 对 bug 发生原因的推断

由于 GitLink 与 trustie 平台有合作关系，因此可能其前端界面在编写时也参考了 trustie 平台的设计，在更改时可能忘记更改“论坛交流”页面的跳转信息，导致该 bug 的发生。

##### bug 反馈

已经在 GitLink 的开发仓库中用“疑修”(issue)反馈了该 bug。

![image](/assets/images/post/SE/gitlink6.png)

系统人员也即时的（在第二天）进行了回复并且也开始了修复工作。

![image](/assets/images/post/SE/gitlink7.png)


#### GitLink--“加入我们”无法显示全部文字且不支持滚动

##### bug 描述

在 GitLink 平台的“关于我们”中的“加入我们”部分，文本框中的文字无法全部显现且不支持滚动操作。

##### bug 展示

![image](/assets/images/post/SE/gitlink8.png)

##### bug 严重性

###### 系统功能

:star::star:

文本框无法显示全部文字的问题阻碍了用户的正常浏览，使得部分信息被隐藏，因此会影响到系统的功能

###### 安全性

该 bug 不会影响用户和系统的安全性。

###### 用户体验

:star::star:

对于想要加入 GitLink 的用户（尤其是想要应聘产品经理与软件测试工程师的人）来说，文本框无法全部显示导致其无法看到岗位的全部要求，因此不能确定自己是否满足岗位需求，这可能导致用户放弃称为 GitLink 的一员。因此该 bug 会影响上述用户的体验。

##### 对 bug 的预期及改进建议

可以从两个角度改进：

1. 设置文本框大小，使其能匹配文字数量
2. 加入文本框滚动功能，使用户可以通过滚动看到下方被遮挡的文字

##### 对 bug 发生原因的推断

其它文本能正常显示说明很可能不是后端的问题，笔者认为比较有可能的原因是前端的文本框的选择问题（没有选择合适尺寸的文本框 / 没有选择可滚动浏览的文本框）。

##### bug 反馈

在 GitLink 的官方代码仓库中新建的“疑修”(issue)。

![image](/assets/images/post/SE/gitlink9.png)

系统人员也在提出 bug 的第二天就进行了即时的修复，如下：

![image](/assets/images/post/SE/gitlink10.png)

#### GitCode--Follow 无反映

##### bug 描述

点击用户下方的 Follow 无响应，提示 `javascript:void(0)` （实际上关注成功，但前端无显示）

##### bug 展示

![image](/assets/images/post/SE/gitlink11.gif)

##### bug 严重性

###### 系统功能

:star::star:

“关注”功能是该平台在社交方面的重要功能之一，无法正常使用会破坏系统功能。

###### 安全性

该 bug 不会影响用户和系统的安全性。

###### 用户体验

:star::star:

该 bug 会给用户带来无法关注他人的错觉，会影响使用体验。

##### 对 bug 的预期及改进建议

希望可以点击 "Follow" 之后该按钮即变为 "Unfollow"。

##### 对 bug 发生原因的推断

由于点击 Follow 后由于实际上关注成功（刷新可见），因此可以证明前端确实向后端发送了关注的请求，且后端也正常处理了，问题应该出在前端在发出请求后没有执行相应动作（将 Follow 变为 Unfollow），或后端没有返回相应的结果，又或者前后端对接有误上。

##### bug 反馈

在 GitCode 的公共仓库中用 Issue 的方式提交了反馈。

![image](/assets/images/post/SE/gitcode4.png)

#### GitCode--在双击后无法正常使用“添加回复”功能

##### bug 描述

在两次点击评论区“添加回复”功能后，该功能无法正常使用，且在每次尝试时均可复现。

具体情况说明：当已经点击两次“添加回复”按钮之后，只有每两次点击之后才会弹出表情选择窗口，之后迅速关闭，无法正常选择表情，且“添加回复”按钮在此过程中一直呈现蓝色（被选中状态，无论是否真的选中）

##### bug 展示

![image](/assets/images/post/SE/gitcode5.gif)

##### bug 严重性

###### 系统功能

:star:

该 bug 会对“添加回复”这一功能的使用造成影响，但由于该功能并不会阻碍 GitCode 的主要功能（如仓库管理等）的正常实现，因此对系统功能的影响不大。

###### 安全性

该 bug 不会影响用户和系统的安全性。

###### 用户体验

:star::star:

该 bug 会给用户带来比较疑惑的使用感受，笔者第一次遇到该 bug 时总以为是自己操作失误（比如点击过快导致窗口快速弹出后消失），但在几次复现之后确定是系统问题。由于添加回复并不是主要功能之一，因此对正常使用 GitCode 不会有太大影响，但可能使得用户产生自我怀疑。

##### 对 bug 的预期及改进建议

“添加回复”功能的目的是为了让用户可以用 emoji 的方式快速对代码 / 仓库进行评价，和旁边的点赞和点踩功能一样，其设计应为单数次点击时功能生效，偶数次点击时候取消点击操作，而并非在第三次及以后的点击时即失效。

##### 对 bug 的发生原因的推断

可能是由于“添加恢复” button 的相关执行动作的逻辑编写有误，导致其无法在两次点击后恢复未点击状态。

##### bug 反馈

在 GitCode 的公共仓库中用 Issue 的方式提交了反馈。

![image](/assets/images/post/SE/gitcode6.png)

#### GitCode--“分析->仓库”中编程语言一栏无显示

##### bug 描述

在仓库中点击“分析”->“仓库”，第一栏“当前代码仓库中使用的编程语言”无显示。（在笔者浏览到的所有仓库中均有该问题）

##### bug 展示

![image](/assets/images/post/SE/gitcode7.png)

##### bug 严重性

###### 系统功能

:star::star:

系统中提供了分析仓库编程语言的功能，在实际使用中却无法显示，会影响到系统的功能。

###### 安全性

该 bug 不会影响用户和系统的安全性。

###### 用户体验

:star::star:

该 bug 会给用户带来不很好的用户体验。这也让我体会到于其放置一个有 bug 的功能，不如干脆取消该功能，因为 bug 会极大的影响用户体验和用户对于该平台的评价。

##### 对 bug 的预期及改进建议

有两种可能的改进方式：

1. 取消代码语言分析功能
2. 修复 bug，呈现完整功能

##### 对 bug 的发生原因的推断

可能是由于前后端连接的问题，也可能是前端无法正常处理后端提供的数据或后端未提供合法的数据。

##### bug 反馈

在 GitCode 的公共仓库中用 Issue 的方式提交了反馈。

![image](/assets/images/post/SE/gitcode8.png)

### 采访

我采访了其他软工课程的 D 同学，她主要是负责前端相关的工作，因此在实际使用中也会比较关注软件前端的设计。她已经是 Gitee 和 GitHub 的用户，对于上述两个产品也有着良好的使用体验，因此这次主要邀请她使用 GitLink 和 GitCode 两个产品，并提出自己的看法。

#### 采访截图

![image](/assets/images/post/SE/gitcode9.png)

![image](/assets/images/post/SE/gitcode10.png)

#### 采访总结

##### GitLink

D 同学尝试在 GitLink 上注册账号，但由于无法收到邮箱验证码（前端无反馈）而失败。体验结束。

> 彼时 GitLink 的工作人员正在回复我关于登录故障的 bug 反馈。笔者怀疑此时开发者正在调试，因此网站的功能无法正常实现。但在采访的最后再次尝试注册时依然无法正常收到验证码，bug 也仍未修复。这说明该产品在部分时间是无法使用的，这会极大的影响用户体验，也会丧失掉小部分潜在用户。

虽然无法注册，D 同学依然在短暂的浏览过程中发现了网站前端的设计问题。（不愧是一个优秀的前端人！）

在此将发现的问题予以展示：（注：少量问题来自 GitLink 的合作社区“确实”(Trustie)）

1. 对齐问题
   ![image](/assets/images/post/SE/gitlink12.png)
2. 间距问题
   ![image](/assets/images/post/SE/gitlink13.png)
3. 重叠问题
   ![image](/assets/images/post/SE/gitlink14.png)

##### GitCode

D 同学尝试使用了 GitCode 的代码管理（建立仓库、Fork、Clone、设置权限等）和社交（Follow, Star, 提 Issue, 换头像等）功能。

发现网页的前端设计还存在瑕疵，如按键在点击过后不会恢复原状，部分按键点击无反应，部分按键无法点击，换头像后前端不能即时有变化等。

除此之外，GitCode 也缺乏即时的消息提醒功能，如当一个用户向另一个用户的仓库提交权限申请时，另一用户并不能在页面上发现该消息，而需要前往个人仓库，点击“项目设置”->“项目成员设置”才能看到，这让权限申请功能变得几乎无法使用（很少有人会经常查看自己项目中设置里的某项细节）。

除此之外，笔者也发现了一些前端怪异的设计，如下：（弹出的窗口并不会自动折叠）

![image](/assets/images/post/SE/gitlink15.gif)

这些奇妙的前端设计给我们的采访带来了很多欢乐。（在实际使用中大概率会给用户带来痛苦）

除此之外，这两个平台也有其优点。D 同学回忆起自己在做编译作业时就苦于找不到国内的平台托管代码（无法登录 GitHub，Gitee 又一时无法正常连接评测机），“虽然 UI 设计的有点丑，但是至少功能还是可以正常使用的。如果知道有这两个平台我肯定会去试试的”，她评价道。这也从侧面反映了两个平台目前还缺少宣传。

### 评价

| 平台名称 | 评价 |
| --- | --- |
| GitLink | 非常不推荐 |
| GitCode | 不推荐 |
| Gitee | 好，不错 |
| GitHub | 非常推荐 |

总的来说，GitLink 和 GitCode 在使用中都遇到了不少问题，虽然他们的开发团队（尤其是 GitLink）在不断的修复 bug，但目前来看，其用户体验还不能算理想。与此相比，Gitee 虽然商业气息浓厚，使用感受却优于上述两个平台，而 GitHub 做为全世界程序员的首选，更是能出色的解决用户的需求。

## 第二部分 分析

> 注：制作时间估计时默认：团队人数 6 人左右，计算机专业毕业生，并有专业 UI 支持。

### GitLink

#### 制作时间估计

目前 GitLink 平台在使用中还存在一些问题，且其功能相较于本次作业中调研的其它平台而言也尚待完善。除此之外，由发现的 bug（“论坛交流”板块“登录”跳转错误）可知，其前端界面很可能是借鉴其它平台完成，因此需要的时间就更少，笔者认为第一版开发时间可以以月为单位计算。由于该平台目前也仍有不少 Issue 提出，因此在第一版开发完成并发布后，还会面临漫长的 bug 修复和完善工作。

#### 优劣对比

相比于本次作业测试的其它网站而言，GitLink 在功能和 Bug 上都做的不太好，因此在这 4 个软件中排名最低。

值得一提的是，笔者出于好奇调查了 GitLink 误跳转至的平台 [Trustie（“确实”）](https://www.trustie.net/homes)，发现其与 GitLink 存在合作关系（虽然两者的用户信息并不通用），事实上，确实平台的“协同开发”功能，就是指 GitLink 提供的服务。

确实平台功能强大，具有协同开发、教学实践、资源共享、众包标注、群体学习、Docker 镜像、容器构建等多个功能，但这也使得其功能过于复杂且设计不统一，平台跳转十分复杂。

更具体的来说，确实平台主页上菜单栏的每一项具体功能都分别对应着不同的合作网站（除了“容器构建”是属于确实平台本身的功能），且其它网站不一定能跳转回确实平台，用户在其它网站中也需要注册新的账户，才可以正常使用功能。具体的跳转方式如下：

![image](/assets/images/post/SE/diff1.png)

这对于初次使用该平台的笔者来说，是十分迷惑的。在并不了解“确实”平台的功能时又另外引入了六个全新的平台，且 UI 设计和账号密码各不相同，功能也各异，如此大的信息量让笔者一时难以接受。也认识到功能多并不一定意味着产品成功，功能之间有内在逻辑，有和谐统一的设计风格和具体的用户指导才是能真正提高用户满意度的关键。

回到 GitLink，该平台也存在上述跳转复杂的问题，且不提登录界面可能跳转至其它平台，GitLink 的“教学实践”中的项目会直接转入一个叫[头歌](https://www.educoder.net/)的平台，且无法返回 GitLink。（这似乎是一个 feature 而不是 bug，却像 bug 一样让笔者迷惑）

总的来说，GitLink 有和头歌、Trustie、OSSEAN、Codepedia、学习空间和 Docker 容器镜像管理系统之间错综复杂的关系，而这些关系在笔者看来并不能提高使用体验，反而会造成困惑。

#### 可提高的方向

我认为 GitLink 平台目前最大的提高方向是提供更好的服务（如部署静态页面，查看 commit 树和用户贡献图等），加大宣传力度。更具体的来说，GitLink 是一个和大学合作建立的平台，也有“教学实践”的功能，因此可以多与学校合作，找到独特的落脚点，为授课和练习提供更加便利的功能（如开设课程，提交作业，代码评价，利用 CI/CD 运行测试等功能），利用平台在教育方面的关系吸引更多用户在该平台上贡献更有质量的代码和课程。

#### bug 发生原因

##### “论坛交流”板块“登录”跳转错误

该 bug 根本上是由于借鉴其他平台的代码而漏改了部分功能。造成该问题发布的原因可能是由于测试人员未进行完整的测试。（可能只是测试了主页或者部分页面的登录功能，而由于自信等原因忽略了“论坛交流”板块的登录功能）

##### “加入我们”无法显示全部文字且不支持滚动

根本上是前端代码的问题，但发布此 bug 的原因可能是测试人员没有做足测试。

### GitCode

#### 制作时间估计

GitCode 也是一个相对较新的平台，于 2021 年末才正式建立品牌。由于目前已有不少可以借鉴的代码管理网站，因此 GitCode 的开发时间也不会过长，我认为和 GitLink 一样，完成其基本功能是可以以月为单位估计的。但在基本代码管理功能之上，GitCode 还提供了 CI/CD, Page，头条等其他功能，这可能需要与其他公司合作，考虑到人与人交流的时间，我认为这些更高级的功能需要更长的时间开发完善。

#### 优劣对比

GitCode 的功能相对齐全，且使用人数虽然远远不及 GitHub 和 Gitee，也有一定规模。且因为是 CSDN 提供的平台，因此其基于 CSDN 的潜在用户数也不容小觑。我认为它在使用体验上优于 GitLink，但由于用户数量较少，有价值的仓库数量也较少，因此排名不如 Gitee 和 Github。

此外，笔者想要额外说明的是，在 UI 的设计上，GitCode 也有不足之处：

在首页和大部分页面中，GitCode 的菜单栏有 8 项，如图：

![image](/assets/images/post/SE/diff2.png)

但在“关于 GitCode” 页面中，菜单栏只有 5 项，且没有了用户信息栏。

![image](/assets/images/post/SE/diff3.png)

导致上述 UI 设计不一致的原因可能是 about 界面使用了和其余界面不同的布局，这虽然在功能上并没有显著的影响，但对于追求 UI 和谐统一的笔者来说还是略有些难受。

#### 可提高的方向

由于有着 CSDN 平台的支持和分流，GitCode 在笔者看来有着不小的发展潜力，可以加大宣传力度，吸引更多优秀的企业或者个人在 GitCode 上贡献高质量的代码，从而吸引更多的开发者入驻，形成良性循环。

#### bug 发生原因

##### 在双击后无法正常使用“添加回复”功能

根本上是由于 button 的 on-click 逻辑编写有误，但该 bug 被发布也有测试人员不够了解用户的原因。（由于单次点击并不会造成该问题，测试人员难以发现 bug，但用户在使用过程中可能由于未看清或点击过快等两次点击“添加回复”按键，从而导致 bug 发生）

##### “分析->仓库”中编程语言一栏无显示 & "Follow" 无反馈

根本上是前后端代码的问题，但发布此 bug 的原因可能是测试人员测试的粗心。

### GitHub & Gitee

#### 制作时间估计

两者都是较为成熟，且用户规模庞大的平台，其制作时间需要以年为单位，不断迭代创新。

#### 优劣对比

两者的功能都极为丰富，在两者中，笔者更喜欢 GitHub 平台，原因是其包含的代码量更大，也更适合个人开发者。Gitee 面向的对象主要是企业和学校，有着更多企业管理方面的支持，对于企业来说，可能会更倾向于 Gitee 提供的服务。

#### 可提高的方向

Gitee 可以打造更多企业相关的功能，和钉钉等企业展开合作，同时也可以开拓个人开发者的市场，利用国内企业的优势发展中国用户。

GitHub 可以打造更丰富的论坛（经验交流与 debug 等），也可以为企业、学校等提供更个性化的服务。

## 第三部分 建议和规划

### 市场概况

#### 市场大小

据 the State of the Developer Nation 报道，截止至 2021 年末，世界上活跃的开发人员约有 2680 万人，这些开发者都是潜在的用户，除此之外，部分非开发者的人员也可能有管理文件等需求，也可能会应用仓库管理平台。实际上，市场的大小可能会以亿为单位（GitHub 的用户就有七千多万）。

#### 用户数量（实际 / 潜在）

| 平台名称 | 实际用户量 |
| --- | --- |
| GitLink | 5 万 |
| GitCode | （“一亿人”的安全代码仓？） |
| Gitee | 800 万 |
| GitHub | 7300 万 |

对于新兴的国内平台 GitCode 和 GitLink 来讲，其面临的竞争压力极大，其潜在用户可能只有部分有特殊需求（如连接外网困难，或是一些校内有开课需要的人。而对于 Gitee 来说，它的定位主要是面向企业，故其潜在用户主要是各大企业和其管理人员。对于 GitHub 来说，它的潜在用户是以全世界为范围的所有需要版本管理和分享资源的用户，大约是以亿为单位。

### 市场现状

目前市场上已经有很成熟的代码仓库管理平台 GitHub 和主要面向企业（和中国用户）的平台 Gitee 和主要面向私服的平台 GitLab。且三者都可以很好的满足绝大多数用户的需求。

且由于代码仓库管理平台的一大优势在于其用户量与优质的代码仓库数量，而目前国内外优秀的企业和开发者都选择在上述三个平台上发布和分享代码，学者的论文中也常有指向 GitHub 仓库的链接。因此新兴的代码仓库管理平台面临的竞争是及其激烈的，如何吸引更多优质用户是其发展的关键。

目前新兴的代码仓库管理平台的定位都相对比较特殊，如学校为了保证代码的安全性，在校内部署搭建的平台（如北航软院的代码仓库管理平台），或面向授课的平台（如 GitLink），又或是面向某地区的平台（如国内的各大平台）。对于目标用户，这些产品可能相比目前流行的产品更具优势。

### 市场与产品生态

| 平台名称 | 核心用户群 |
| --- | --- |
| GitLink | 国内创新型软件、军地开源社区和关键领域、高校教育 |
| GitCode | CSDN 用户，有教学研需求的本土用户 |
| Gitee | 优秀本土开源作者、企业和高校 |
| GitHub | 世界优秀开发者、企业和高校等 |

### 产品规划

以 GitCode 为例

#### NABCD 分析

| 表示 | 名称 | 分析 |
| --- | --- | --- |
| N | Need, 需求 | 国内用户有中文代码仓库和安全管理代码的需求，除此之外，高校也有教学研究需求，国家也有为开源项目提供国产化平台，鼓励本土企业创新发展的需求 |
| A | Approach, 做法 | 与 CSDN 紧密合作，借助 CSDN 平台的用户量吸引更多优秀开发者。还可以开发移动端 app，使移动端浏览代码更加便利 |
| B | Benefit, 好处 | 本土化，安全，中文环境，有大体量的问答社区支撑 (CSDN) |
| C | Competitors, 竞争 | 竞争对手主要是国内其他的代码管理平台，目前 GitCode 的用户量远远不及 Gitee，但可以借助 CSDN 的引流优势，加强宣传，为开发者提供更便捷优惠的使用体验来获得更大的市场。 |
| D | Delivery, 推广 | 公众号文章、校企合作、在 CSDN 中加入跳转按钮，引导博客作者或问答者将代码 / 代码片段分享到 GitCode 上，为优秀开发者提供奖励（现金、礼品或权益等） |

#### 配置角色

> 如果你是项目经理，可以招聘6个人，并且有4个月的时间，你认为应该如何配置角色(开发，测试，美工等等) 才能在第16周如期发布软件的改进版本，并取得预想中的成绩。

| 人数 | 角色 | 任务 |
| --- | --- | --- |
| 4 | 开发 | 前后端开发、单元测试 |
| 2 | 测试 | 测试网站功能 |
| 1 | 美工 | 原型设计、网站美化、制作宣传海报等 |

开发者中善于沟通协调者担任项目 PM，其开发任务可以相对减少，在团队协调方面的任务较多。

#### 详细规划

> 请为你的团队设计16个周期每周的详细规划。

| 周数 | 任务 |
| --- | --- |
| 1 | 调研市场、用户需求，了解当前产品优劣和竞争优势、制定需求 | 
| 2 | 原型设计，确定 API 接口，讨论方案的可行性和分工 |
| 3-5 | 在原网站的基础上更新主要功能 |
| 6 | 评估上一阶段成果，根据可行性适当修改需求 |
| 7-10 | 进行第二阶段开发，添加更多功能 |
| 11 | 基本功能测试，完善细节（UI 设计是否和谐统一等） |
| 12-13 | 压力测试和更全面的测试 |
| 14-15 | 招募用户进行内测，根据用户反馈完善产品 |
| 16 | 发布产品 |
---
layout: post
title: Operating System
subtitle: 操作系统课程笔记
categories: ComputerArchitecture
tags: note system operating-system
---

# 操作系统

## 引论

### 基本概念

#### 计算机体系结构中的接口

* UI
  * 用户接口 User Interface
* API
  * 应用程序界面 Application Program Interface
* ABI
  * 应用程序二进制接口 Application Binary Interface
* ISA
  * 工业标准结构 Industry Standard Architecture

### 历史

`史前阶段`-> `批处理`->`分时`->`现代 OS`->`网络化 OS /分布式 OS`

#### 批处理系统

加载在计算机上的一个系统软件，使得计算机能够**自动**的，**成批**的处理一个或多个用户的作业。

##### 联机批处理系统

作业的输入 / 输出由**CPU**来处理

> 克服人机矛盾，提高计算机利用率
>
> 存在高速主机与慢速外设的矛盾

##### 脱机批处理系统

作业的输入 / 输出**脱离主机控制**

> 克服高速主机与慢速外设的矛盾
>
> 等待 IO 时 CPU 处于空闲状态

#### 多道程序系统

允许多个程序同时放入内存并运行

> 使 CPU 得到充分利用，改善 I/O 设备和内存的利用率，提高整个系统的资源利用率和内存的利用率

![image](https://img2020.cnblogs.com/blog/2324053/202104/2324053-20210426104110847-2086237697.png)

**宏观上并行，微观上串行**

#### 多道批处理系统

* **多道**： 系统可同时容纳多个作业
* **成批**： 系统运行过程中，不允许用户与其它作业发生交互作用

#### 分时系统

处理机的运行时间被分成很短的时间片，按时间片轮流把处理机分配给联机作业使用。

* 多路性
* 交互性
* 独立性
* 及时性

#### 分布式系统

整个系统中有一个全局的操作系统

#### 实时系统

* 及时响应
* 高可靠性和安全性
* 系统的整体性强
* 交互会话活动较弱

### 计算机硬件简介

#### 内存

#### Cache

#### 磁盘

### 操作系统的特征

* 并发
* 共享
  * 互斥共享
  * 同时访问
* 虚拟
* 异步性

## 系统引导

### 基础知识

#### 启动 Bootstrapping

#### Bootloader

系统加点后运行的第一段软件代码，是在操作系统内核运行前运行的一段小程序。

#### MIPS 的基本地址空间

![image](https://img2020.cnblogs.com/blog/2324053/202104/2324053-20210426130707406-741030482.png)

|  命名   |            地址             |                         功能                         | MMU  | Cache |
| :-----: | :-------------------------: | :--------------------------------------------------: | :--: | :---: |
| `kuseg` | `0x0000 0000 - 0x7FFF FFFF` |                    用户态可用地址                    |  √   |   √   |
| `kseg0` | `0x8000 0000 - 0x9FFF FFFF` |                 存放**操作系统核心**                 |  ×   |   √   |
| `kseg1` | `0xA000 0000 - 0xBFFF FFF`  | I/O 寄存器，**唯一在系统重启时能正常工作的地址空间** |  ×   |   ×   |
| `kseg2` | `0xC000 0000 - 0xFFFF FFFF` |                  可供管理态程序使用                  |  √   |   √   |

## 存储器管理

### 存储器管理基础

#### 存储层次结构

寄存器 》 高速缓存 》 主存 》 外存

#### 大小尾端

提到体系结构时，经常遇到大小尾端的概念，这里做个总结。
big endian：大尾端，也称大端（高位）优先存储。
little endian：小尾端，也称小端（低位）优先存储。
如下00001000 00000100 00000010 00000001的存储

```
大尾端： 00001000 00000100 00000010 00000001
        addr+0    addr+1     addr+2   addr+3   //先存高有效位（在低地址）
小尾端： 00000001 00000010 00000100 00001000
        addr+0    addr+1     addr+2   addr+3   //先存低有效位（在低地址）
```

比如 short int a = 0x1234
大尾存放时：
偏移地址   存放内容
0x0000    0x12
0x0001    0x34

小尾存放：
偏移地址   存放内容
0x0000    0x34
0x0001    0x12

#### 程序的装入和链接

##### c 语言编译+链接过程

* 编译
  * `.c`->`.o`
* 链接
  * 将 `.o` 文件和库文件链接到一起，形成最终的可执行文件
* 重定位
  * 填写之前未填写的地址的过程

##### `gcc` 调用包含的几个工具

* `cc1`: 预处理器和编译器
* `as`: 汇编器
* `collect2`: 链接器

##### `ELF(Executable and Linkable Format)`-可执行文件格式

![image](https://img2020.cnblogs.com/blog/2324053/202104/2324053-20210426132323158-80020968.png)

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210503164950768-1931582126.png)

* `.text`
  * **正文**，存放程序的执行指令
  * 只读
  * 包括命令语句和一些只读的常数变量，如字符串常量等
* `.data`
  * **数据段**，存放程序内存映像的初始化数据
  * 存放已初始化的全局变量
  * 属于静态内存分配
* `.bss`
  * **静态段**，存放程序内存映像未初始化数据，不沾文件空间
  * 存放未初始化的全局变量
* `heap`
  * **堆**
  * 先进先出
  * 存放程序中被动态分配的内存段
  * 属于动态内存分配
* `stack`
  * **栈**
  * 先进后出
  * 存放程序临时创建的局部变量
  * 属于动态内存分配
  * 在函数调用时，其参数会被压入发起调用的进程栈中，并且等调用结束后，函数的返回值也会被存放回栈中。

###### ELF Header

![image](https://img2020.cnblogs.com/blog/2324053/202104/2324053-20210426133127100-816057673.png)

##### Program Header Table



##### Section Header Table



#### 程序的装载和运行

##### 程序的装载

* `shell` 调用 `fork()` 系统调用，创建出一个子进程

* 子进程调用 `execve()` 加载 program

  * 加载器根据 ELF 文件中的 segment 相关信息完成加载

    > 一个 `segment` 在文件中的大小 ≤ 内存中的大小
    >
    > 通过 **补零** 使其达到其在内存中应有的大小

##### 程序的运行

* 系统调用 `execve`

### 基础内存管理

#### 单道程序的内存管理

##### 分配方式

* 直接指定
* 静态分配
* 动态分配

##### 优缺点

* 优点
  * 无需地址翻译，程序运行速度快
* 缺点
  * 比物理内存大的程序无法加载，因而无法运行
  * 造成资源浪费

#### 多道程序的内存管理

##### 分区式匹配

###### （固定式）静态分区分配

* 分区大小

  * 相等

    适合多个相同程序的并发执行

  * 不等

* 队列数目

  * 单队列

    ![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210503195048476-1795308777.png)

  * 多队列

    ![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210503195110247-16117104.png)

* 优缺点

  * 优点

    易于实现，开销小

  * 缺点

    内碎片造成浪费，分区总数固定，限制了并发执行的程序数目

  * 数据结构

    分区表——记录分区的大小和使用情况

###### （可变式）动态分区分配

* 优缺点

  没有内碎片，有外碎片

#### 闲置空间管理

##### 位图表示法

给每个单元赋予一个空位，用来记录单元是否闲置。

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210503201329849-1037490167.png)

##### 分区表示法

将分配单元按照是否闲置链接起来。

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210503201353524-914507544.png)

#### 可变分区的管理

已分配分区表和未分配分区表

#### ==分区分配算法==

##### 首次适应算法(First Fit)

从空白区域链始端开始查找，选择第一个满足要求的空白块。

##### 下次适应算法 (Next Fit)

从上一次查找结束的地方开始，选择第一个满足要求的空白块。

##### 最佳适应算法 (Best Fit)

总是寻找大小最接近作业要求的存储区域

##### 最坏适应算法(Worst Fit)

总是寻找最大的空白分区

##### 基于索引的搜索分配算法

##### 快速适应算法

##### 伙伴系统

无论已分配分区或空闲分区，其大小均为 2 的 k 次幂。将空闲分区分成相等的两个分区，称为一对伙伴。

#### 系统中的碎片

##### 内碎片

* 分配给作业的存储空间中未被利用的部分
* 无法被整理，只能在作业完成后得到释放

##### 外碎片

* 分区和分区之间存在的碎片

* 消除方法： **紧凑技术**

  > 通过移动作业把多个分散的小分区拼接成一个大分区

#### 分区的存储保护

##### 界限寄存器方法

* 上下界寄存器方法
* 基址，限长寄存器方法

##### 存储保护键方法

#### 分区管理

> 如何实现小内存运行大作业？

##### ==覆盖==

* 把一个程序划分为一系列功能相对独立的程序段，让执行时不要求同时装入内存的程序段组成能一组，共享主存的同一个区域。
* 要求作业各模块之间有明确的调用结构， **程序员要向系统指明覆盖结构**，然后由操作系统自动完成覆盖
* 缺点：**对用户不透明，增加用户负担**
* 主要对 **同一个作业或程序进行**

##### ==交换==

* 把暂时不用的某个（或某些）程序及其数据的部分或全部从主存移动到辅存中去。
* 优点：增加并发运行的程序数目，给用户提供适当的响应时间；编写程序时不影响程序的结构
* 缺点：对换入和换出的控制增加处理机开销；程序整个地址空间都进行传送，没有考虑到执行过程中地址访问的统计特性
* 主要在作业和程序间进行

### 页式内存管理

#### 基本原理

##### 程序

* 静止
* 一个程序可以作为多个运行进程的运行程序

##### 进程

* 动态
* **计算机系统有限资源的基本单位**
* 能真实的描述并发
* 由生存周期
* 一个进程可以运行多个程序
* 一个进程可以创建其它进程

##### 作业

* 用户需要计算机完成的某项任务，要求计算机所做工作的集合
* 一个作业通常包括程序、数据和操作说明书
* 作业完成阶段：
  * 提交
  * 收容
  * 执行
  * 完成
* 一个作业可以由多个进程组成，且至少由一个进程组成，反之不成立

#### 基本概念

##### 页

把每个地址空间分成一些**大小相等**的片，称作 **页面** 或 **页**，各页从 0 开始编号

##### 存储块

把主存的存储空间页分为 **与页面大小相等的** 片，称作 **存储块** 或 **页框 (Frame)**

##### 页表

方便在内存中找到进程每个页面所对应的块。进程**逻辑地址空间**中的每一页，在页表中都有一个页表项

##### 地址变换

##### 多级页表

> 为什么要采用多级页表？（aka. 一级页表的问题）
>
> 因为页表的存放要求**地址连续**，若逻辑地址空间很大，则页表就很大，占用的连续存储空间就很大，实现困难，因此可以采用多级页表（或者动态调入页表）的方式

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505150415858-1017472083.png)

##### 快表 (TLB, Translation Lookaside Buffer)

块表是一种特殊的高速缓冲存储器，内容是页表中的一部分或全部内容

CPU 产生逻辑地址的页号，首先在块表中查找，若命中就找出其对应的物理块，若未命中，再到页表中找出其对应的物理块。

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505150640919-918643565.png)

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505150728433-1811027996.png)

#### 纯分页系统 (Pure Paging System)

* 不具备页面对换功能，不支持虚拟存储器功能
* 在调度一个作业时，必须把它的所有页一次装到主存的页框里
* 优缺点：
  * 优点：没有外碎片，每个内碎片不超过页大小，一个程序不必连续存放
  * 缺点：程序全部装入内存

##### 地址结构

###### 逻辑地址

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505145630757-169251152.png)

###### 物理地址

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505145654609-1935493561.png)

##### 数据结构

###### 进程页表

每个进程有一个页表，描述该进程占用物理页面及逻辑排列顺序

###### 物理页面表

每个系统有一个物理页面表，描述物理内存空间的分配和使用情况

* 位图，空闲链表

###### 请求表

每个系统有一个请求表，描述系统内各个进程页表的位置和大小

##### 页表

访问一个数据需要访问内存 **2** 次（页表一次，内存一次）

页表的基址和长度由页表寄存器给出

##### 地址转换机构

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505150042395-247837520.png)

#### 页面大小

* 若页面较小
  * 减少内碎片和总的内存碎片
  * 每个进程的页面数增多，页表程度增加，占用内存增大
  * 页面换进换出速率降低
* 若页面较大
  * 每个进程页面数减少，页表长度减少，占用内存较小
  * 页面换进换出速度提高
  * 增加页内碎片，不利于提高内存利用率

#### 页表类型

##### 哈希页表

用虚拟页码作为哈希值。

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505180445459-271419957.png)

##### 反置页表 (inverted page table)

* 反置页表是根据该进程在内存中的**物理页面号**来组织，其表项内容是逻辑页号 `p` 及隶属进程标识符 `pid`
* 反置页表的大小**只与物理内存的大小有关**，与逻辑空间大小和进程数无关。
* ![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505185222076-641500225.png)
* 可能需要查找整个表来寻找匹配，可以通过使用哈希表来限制页表条目或者加入 TLB 来改善

#### 页共享

最好的实现方式：分段存储管理

### 段式内存管理

#### 基本原理

> * **方便编程**
>
>   一个作业由多个代码段和数据段组成，用户可以根据名字访问到对应的程序段和数据段
>
> * **信息共享**
>
>   共享以信息的逻辑单位为基础
>
>   以 **页** 为存储信息的物理单位，以 **段** 为存储信息的逻辑单位
>
> * **信息保护**
>
>   以信息的逻辑单位进行保护
>
> * **动态增长**
>
>   实现某些段的不断增长
>
> * **动态链接**
>
>   把主程序和需要用到的目标程序链接起来

##### 分段地址空间

一个段可以定义为一组逻辑信息，每个作业的地址空间由一些分段构成，每个段有自己的名字，且都是一段连续的地址空间，首地址为 0

##### 地址结构

###### 段表

* 段表保存在内存中，记录了段与内存位置的对应关系
* 访问数据需要先访问段表一次，再访问内存一次
* 逻辑地址 = 段号 + 段内偏移

#### 地址变换

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505203952551-484047019.png)

#### 段共享

**可重入代码**（纯代码）

> 允许多个进程同时访问的代码，只读，各个进程执行的代码完全相同

#### 优缺点

##### 优点

* 易于实现段的共享，对段的保护十分简单

##### 缺点

* 变换地址花费时间，且需要为段表提供附加的地址空间
* 为满足分段的动态增长和减少外零头，要采用拼接手段
* 辅存管理不定长度的分段较为困难
* 分段的最大尺寸受到主存可用空间的限制

#### 与页式管理优缺点对比

* 分页的地址空间是单一的线性地址空间，而分段的地址空间是二维的
* **页** 是信息的 **物理单位** ，而 **段** 是信息的 **逻辑单位**
* 页大小固定，段大小不固定
* 分页用户不可见，分段用户可见

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505204531879-1275370754.png)

#### 段页式管理

##### 基本思想

用 **分段** 方法来分配和管理虚拟存储器，用 **分页** 的方法来分配和管理实存储器

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505204702764-1138006488.png)

##### 实现原理

地址组成

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505204741937-816298569.png)

**一个进程一个段表，每个段一个页表**

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505204828444-24496586.png)

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505204905163-1147211281.png)

### 虚拟存储管理

#### 局部性原理

#### 基本原理

* 在程序装入时，不必将其完全读到内存，而只需要将当前需要执行的不分页或段读入内存
* 在程序运行过程中，若需要更多的页面/段，则产生缺页中断，通知操作系统将其调入到内存中
* 操作系统将内存中暂时不用的页或段保存在外存中

#### 虚拟内存

虚拟内存为每个进程提供了一个 **一致的、连续的、私有的** 地址空间。

* 离散性

  物理内存分配不必连续

* 多次性

  作业分多次调入内存

* 对换性

  允许作业在运行过程中换进换出

* 虚拟性

  不考虑物理内存上可用的空间和数量

##### 优点、代价和现实

###### 优点

* 在较小的物理内存中执行较大的程序
* 可在内存中容纳更多的程序并发执行
* 不影响编程时的程序结构
* 提供给用户大于物理内存的虚拟空间

###### 代价

* 牺牲 CPU 处理时间和内外存的交换时间

###### 限制

虚拟内存的最大容量由计算机的地址结构决定

#### 请求式分页

在分页（分段）系统的基础上，增加了请求调段（页）的功能

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505214835157-1263893749.png)

##### 请求式分页管理的页表

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505220106830-290017426.png)

* 驻留位
  * 1: 该页在内存当中
  * 0: 该页当前还在外存当中
* 保护位
  * 只读、可写、可执行
* 修改位
  * 表明此页在内存中是否被修改过
* 访问（统计）位
  * 用于页面置换算法

#### 基本概念

##### 逻辑空间（虚拟空间）

通过连接器，将构成进程所需要的程序及其运行所需要的环境，按照某种规则装配链接而形成的一种规范格式。

格式按字节从 0 开始编址所形成的空间

##### 虚拟地址空间 & 虚拟存储空间

一个进程虚拟地址空间是进程在内存中存放的逻辑视图

一个进程的虚拟地址空间大小 = 虚拟存储空间大小

##### 交换分区（交换文件）

在物理内存不够的情况下，将内存中的数据存到硬盘的交换空间中，从而获得更大的空间

##### 地址映射问题

* 在每个进程创建加载时，内核只是为进程创建了虚拟内存的布局，而并没有把信息拷贝到对应的地址上。等程序运行时，才通过缺页异常拷贝数据
* *用户可执行文件* 及 *共享库* 都是以文件的形式存在磁盘中，初始时在页表中的类型为 *file backed* 
* *堆*  和 *栈* 在磁盘上没有对应的文件，页表中类型为 *anonymous*
* 未分配部分没有对应的页表项，只有在申请时才建立相应的页表项

#### 虚拟存储器的管理

##### 页面调入

###### 预调页

事先调入页面

###### 按需调页

仅当需要的时候调入页面

##### 页错误处理

1. **现场保护**

   陷入内核态，保存必要信息

2. **页面定位**

   查找发生中断的虚拟页面

3. **权限检查**

   检查有效性及安全保护位

4. **新页面调入**

5. **旧页面写回**

6. **新页面调入**

7. **更新页表**

8. **恢复现场**

9. **继续执行**

##### ==置换问题==

###### 最优置换（Optimal）

理想情况，无法被实现

最优置换会将内存中的页 P 置换掉，如果页 P 满足：从现在开始到未来的某时刻再次需要页 P，这段时间最长。也就是 OPT 算法会置换掉未来最久不被使用的页。

###### 先进先出算法（First-in First-out）

* Belady 现象

  > 所谓 Belady 现象是指：在分页式虚拟存储器管理中，发生缺页时的置换算法采用FIFO（先进先出）算法时，如果对一个进程未分配它所要求的全部页面，有时就会出现分配的页面数增多但缺页率反而提高的异常现象。

* 改进版本：

  * **第二次机会算法（Second Chance）**
  
    如果被淘汰的数据之前被访问过，则给其第二次机会（Second Chance）
  
    每个页面会增加一个**访问标志位**，用于标识此数据放入缓存队列后是否被再次访问过。
  
    * A是FIFO队列中最旧的页面，且其放入队列后没有被再次访问，则A被立刻淘汰；否则如果放入队列后被访问过，则将A**移到FIFO队列头，并且将访问标志位清除**。
    * 如果所有的页面都被访问过，则经过一次循环后就会按照FIFO的原则淘汰。
  
  * **Clock** 算法，最近未使用算法（NRU, Not Recently Used）
  
    * 如果没有缺页错误，将相应的页面访问位置1，指针不动
    * 产生缺页错误时，当前指针指向C，如果C被访问过，则清除C的访问标志，并将指针指向D；
    * 如果C没有被访问过，则将新页面放入到C的位置, 置访问标志，并将指针指向D。
  
  ![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505223735498-1088842058.png)

###### 最近不适用页面置换算法（Least Recently Used Replacement）

* 访问第 i 页时先将该页所在的行置 1，所在列清 0，将行编号最小的那一页置出去

* 缺点：空间开销太大

* 改进算法：

  * **老化算法（Aging）**

    位每个页面设置一个移位寄存器，并设置一位访问位 R，每隔一段时间，所有寄存器右移 1 位，并将 R 的值从左移入

##### 最小物理块数的问题

##### 分配问题

##### ==抖动问题 (Thrashing)==

随着驻留内存的进程数目增加，或者说进程并发水平(multiprogramming level)的上升，处理器利用率先是上升，然后下降。

> 每个进程的驻留集不断减小，当驻留集小于工作集后，缺页率急剧上升频繁调页使得调页开销增大。OS要选择一个适当的进程数目，以在并发水平和缺页率之间达到一个平衡。

解决办法：局部置换策略、工作集算法、预留部分页面、挂起若干进程

#### 页面置换

#### 内存保护

### 存储管理实例

### 页目录自映射

#### 基础知识

多级页表

| 10         | 10       | 12       |
| ---------- | -------- | -------- |
| 页目录偏移 | 页表偏移 | 页面偏移 |

| 空间     | 大小 |
| -------- | ---- |
| 虚拟空间 | 4 GB |
| 页表     | 4 MB |
| 页面     | 4 KB |
| 页目录   | 4 KB |
| 页表项   | 4 B  |

$4G$ 空间对应 $2^{20}$ 个页面

$4M$ 页表空间对应 $2^{10}$ 个页表，其中每个页表对应 $2^{10}$ 个页面，即 $2^{22}B = 4MB$ 的内存空间

$4K$ 页目录空间对应 $2^{10}$ 个页目录项，其中每个页目录项对应一个页表，从而对应 $2^{10}$ 个页面，即 $2^{22}B = 4MB$ 的内存空间

#### 背景

![img](http://cscore.net.cn/assets/courseware/v1/384b0b746c79c840e9c3f4ff5b4b0b0f/asset-v1:Internal+B3I062140+2020_T2+type@asset+block/lab2-pic-1.jpg)

![img](http://cscore.net.cn/assets/courseware/v1/79285678df7ddb18b4761b7d88a87de9/asset-v1:Internal+B3I062140+2020_T2+type@asset+block/lab2-pic-2.jpg)

从一级页表、二级页表中获得的地址都是**物理地址**

![](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505230554161-398323370.png)

页目录中有一条 PTE 指向自身的物理地址

![/////////////////////////////////////////////////////////////////////////////////////////////////////////////](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210505230701031-498549903.png)

#### 计算

##### 符号及含义

| 符号                 | 含义                   |
| -------------------- | ---------------------- |
| $PT$                 | Page Table，页表       |
| $PT_{base}$          | 页表基址               |
| $PD$                 | Page Directory，页目录 |
| $PD_{base}$          | 页目录基址             |
| $PDE_{self-mapping}$ | 自映射页目录项         |

##### 公式

$$
PD_{base} = PT_{base} | (PT_{base}) >> 10 \\
PDE_{self_mapping} = PT_{base} | (PT_{base}) >> 10 | (PT_{base}) >> 20
$$



##### 由二级页表初地址求页目录地址

我们知道，这 4M 空间的起始位置也就是第一个二级页表对应着页目录的第一个页目录项，同时，由于 1M 个页表项和 4G 地址空间是线性映射，不难算出 0x7fc00000 这一地址对应的应该是第 (0x7fc00000 `>>` 12) 个页表项，这个页表项 也就是第一个页目录项。一个页表项 32 位，占用 4 个字节的内存，因此，其相对于页表起始地址 0x7fc000000 的 偏移为 (0x7fc00000 `>>` 12) * 4 = 0x1ff000 ，于是得到地址为 0x7fc00000 + 0x1ff000 = 0x7fdff000 。也就是 说，页目录的虚存地址为 0x7fdff000。

## 进程与线程

### 进程概念的引入

#### 基本概念

##### 并发与并行

###### ==并发 Concurrent==

在某一特定时间，两个活动无论是否在同一个处理机上运行，只要两个活动都处于各自起点和重点之间的某一处，则称两个活动是并发执行的。

多个任务在单处理器或多处理器中分时运行

###### ==并行 Parallel==

在同一时间度量下同时运行在**不同**处理机上

多个任务在多处理器上同时运行

##### 前驱图

有向无循环图，图中每个结点表示一条语句，一个程序段或进程，结点间的有向边表示语句或程序段的执行顺序

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210506082359425-1712882056.png)

如图，表示 AB 的执行顺序：先 A 后 B，可写成 $A\rightarrow B$

##### 程序并发执行时的特征

* 间断性
* 非封闭性
* 不可再现性

##### 竞争

* **定义**： 多个进程在读写同一个共享数据时结果依赖于它们执行的相对时间
* **条件**： 多个进程并发访问和操作同一数据且执行结果与访问的特定顺序有关
* **并发进程的无关性——Bernstein 条件**

##### Bernstein 条件

**判断程序并发执行结果是否可再现的充分条件**

两个进程 $S_1$ 和 $S_2$ 可并发，当且仅当下列条件同时成立
$$
R(S_1) \cap W(S_2)  = \empty \\
W(S_1) \cap R(S_2)  = \empty \\
W(S_1) \cap W(S_2)  = \empty \\
$$
其中 $R(S_i)$ 表示 $S_i$ 的读子集，其值在 $S_i$ 中被引用的变量的集合

其中 $W(S_i)$ 表示 $S_i$ 的读子集，其值在 $S_i$ 中被改变的变量的集合

##### ==临界资源==

一次**仅允许一个进程访问**的资源称为 **临界资源**

##### 临界区

每个进程中访问临界资源的那段代码称作 **临界区**

##### 进程

###### 定义

==进程是程序在一个数据集合上运行的过程，是系统进行**资源分配和调度**的一个独立单位==

###### 性质

* 动态性
* 并发性
* 独立性
* 异步性

###### 组成部分

* 代码
* 数据
* PC 值
* 一组通用寄存器的当前值、堆、栈
* 一组系统资源（如打开的文件）

###### 利弊

* 利：提高效率
* 弊：空间、时间开销

###### 与程序的关系

通过多次执行，一个程序可以对应多个进程；通过调用关系，一个进程可以包括多个程序

### 进程状态与控制

#### 进程控制

##### 进程创建

* 创建方式
  * 提交批处理作业
  * 用户登录
  * 由 OS 创建，以向用户提供服务
  * 由已存在的进程创建

##### ==进程状态==

###### 就绪 (Runnable)

###### 执行状态 (Running)

###### 阻塞状态 (Not Runnable / Blocked)

* ![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210506084509403-2109850253.png)
  * 用户退出登录
  * 进程执行中断服务请求
  * 出错 or 失败
  * 正常结束
  * 给定时限到

#### 原语

==若干条指令组成，**连续不可分割**，必须在内核态下执行，且常驻内存==

##### 创建原语 fork, exec

###### fork

* 在 **父进程** 中，fork 返回新创建**子进程**的进程 id
* 在 **子进程** 中，fork 返回 **0**
* 若出现错误，fork 返回一个负值

##### 撤销原语 kill

#### 进程控制块 PCB(Process Control Block)

一个与动态过程相联系的数据结构，记载了进程的外部特性（名字、状态）以及与其他进程的联系（通信关系），还记录了进程所拥有的各种资源。进程控制块是进程存在的标志。

##### 内容

* 进程标识符
* 程序和数据地址
* 现行状态
* 现场保留区
* 互斥和同步机制
* 进程通信机制
* 优先级
* 资源清单
* 链接字
* 家族关系
* 其它...

##### 组织形式

* 线性表
* 链接方式
* 索引方式
  * 就绪索引表
  * 阻塞索引表

#### 进程上下文切换 (Process Context Switch)

* 由调度器执行
* 保存进程执行断点
* 切换内存映射 （页表基址、flush TLB）

#### 陷入 / 退出内核 (Mode Switch)

* CPU 状态改变
* 由中断、异常、Trap 指令（系统调用）引起
* 需要保存执行现场（寄存器，堆栈等）

### 线程概念的引入

#### 可执行单元——线程 (Thread)

#### ==与进程的区别==

进程是 **资源拥有者**

线程是 **可执行单元**，将资源与计算分离，提高并发效率

**进程** 拥有：虚空间、进程映像、处理机保护、文件、I/O 空间

**线程** 拥有：运行状态、保存上下文（程序计数器）、执行**栈**、资源共享机制

#### 引入目的

* 减少进程切换的开销
* 提高进程内的并发程度
* 共享资源

### 线程的实现方式

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210506090740433-1906429894.png)

#### ==用户级线程 (User Level Threads, ULT)==

##### 优点

* 线程切换与内核无关
* 线程的调度由应用决定，容易进行优化
* 可运行在任何支持线程库的操作系统上

##### 缺点

* 系统调用会回引起阻塞，内核会因此阻塞 **所有** 相关的线程
* 内核只能将处理机分配给进程，无法实现一个进程中多个线程的并行执行
* CPU 调度以 **进程** 为单位

#### ==内核级线程 (Kernel Level Threads, KLT)==

##### 优点

* OS 可感知
* 内核线程执行系统调用时，只导致该线程被中断
* CPU 调度以 **线程** 为单位

##### 缺点

* 线程创建、撤销、调度均需要 OS 内核提供支持

#### 混合实现

* 线程在用户空间创建和管理
* 实现从用户空间线程到内核空间线程的映射

## 进程同步

### 同步与互斥问题

#### 进程互斥（间接制约关系）

* 两个及其以上的进程不能同时进入同一组共享变量的临界区域

#### 进程同步（直接制约关系）

* 使得程序的执行具有可再现性的过程
* 是一种刻意安排的直接制约关系

#### 临界区

**空闲让进，忙则等待，有限等待，让权等待**

1. 没有进程在临界区时，想进入临界区的进程可以进入
2. 任何两个进程都不能同时进入临界区 (Mutual Exclusion)
3. 当一个进程运行在它的临界区外面时，不能妨碍其他进程进入临界区
4. 任何一个进程进入临界区的要求应该在有限的时间内得到满足

### 基于忙等待的互斥方法

### 基于信号量的方法

#### ==信号量 (Semaphore)==

##### 定义

一个确定的二元组 (s, q)，其中 s 是一个具有非负值的整型变量， q 是一个初始状态为空的队列

##### 分类

###### 二元信号量

取值仅为 0 和 1，主要用作实现互斥

###### 一般信号量

取值为可用物理资源的综述，用于进程间写作同步问题

##### P (proberen, test)

```c
P(S):
	while S <= 0 do skip;
	S := S - 1;
V(S):
	S := S + 1;
```

##### V (verhogen, increment)

### 基于管程的同步与互斥

### 进程通信的主要方法

### ==经典的进程同步与互斥问题==

#### 生产者-消费者问题

```c
mutex = Semaphore(1); // provides exclusive access to the buffer
item = Semaphore(0); // the number of items in the buffer
spaces = Semaphore(buffer.size()); // the number of consumer threads in queue
// event is a local variable, each threads has its own version

consumer() {
	P(item);
	P(mutex);
		event = buffer.get(); 
    V(mutex);
    V(spaces);
    event.process();
}

producer() {
    P(spaces);
    P(mutex);
    	buffer.add(event);
    V(mutex);
    V(item);
}

main() {
    Cobegin
        producer();
    	consumer();
    Coend
}
```

#### 读者-写者问题

```c
// a solution that may cause starvation
int readers = 0; // keeps track of how many readers are in the room
mutex = Semaphore(1); // provides exclusive access to roomEmpty
roomEmpty = Semaphore(1);

writer() {
    P(roomEmpty);
    	// critical section for writers
    V(roomEmpty);
}

reader() {
    P(mutex);
    	readers += 1;
    	if (readers == 1) { // first in locks
            P(roomEmpty);
        }
    V(mutex);
    // critical section for readers
    P(mutex);
    	readers -= 1;
    	if (readers == 0) { // last out unlocks
    		V(roomEmpty);
        }
    V(mutex);
}

main() {
    Cobegin
        writer();
    	reader();
    Coend
}
```

```python
# no-starve readers-writers hint
class Lightswitch:
    def __init__(self):
        self.counter = 0
        self.mutex = Semaphore(1)
    def lock(self, semaphore):
        P(self.mutex)
        	self.counter += 1
            if self.counter == 1:
                P(semaphore)
        V(self.mutex)
    def unlock(self, semaphore):
        P(self.mutex)
        	self.counter -= 1
            if self.counter == 0:
                V(semaphore)
        V(self.mutex)
        
readSwitch = lightswitch() # how many readers are in the room, locks 'roomEmpty' when the first reader enters and unlocks it when the last reader exits.
roomEmpty = Semaphore(1)
turnstile = Semaphore(1) # a turnstile for readers and a mutex for writers

def writer():
    P(turnstile)
    P(roomEmpty)
        # critical section for writers
    V(turnstile) # unblocks a waiting thread
    V(roomEmpty)
    
def reader():
    P(turnstile) # if no writer is waiting
    V(turnstile)
    readSwitch.lock(roomEmpty)
    	# critical sections for readers
    readSwitch.unlock(roomEmpty) # unblocking the waiting writer
```

```python
# writer-priority readers-writers hint, possible for readers to starve
readSwitch = Lightswitch()
writeSwitch = Lightswitch()
noReaders = Semaphore(1)
noWriters = Semaphore(1)

def read():
    P(noReaders)
    readSwitch.lock(noWriters)
    V(noReaders)
    # critical section for readers
    readSwitch.unlock(noWriters)
    
def writer(): # when a writer is in the critical section it holds both noReaders and noWriters
    writerSwitch.lock(noReaders) # allowing multiple writers to queue on noWriters, but keeping noReaders locked while they are there
    P(noWriters)
    # critical section for writers
    V(noWriters)
    writeSwitch.unlock(noReaders)
```

```python
# no-starve mutex hint <Morris's algorithm>
room1 = room2 = 0 # how many threads are in the waiting rooms
mutex = Semaphore(1) # help protect the counters
t1 = Semaphore(1) # turnstile
t2 = Semaphore(0) # turnstile

P(mutex)
	room1 += 1
V(mutex)

P(t1)
	room2 += 1
    P(mutex)
    room1 -= 1
    
    if room1 == 0:
        V(mutex)
        V(t2)
    else:
        V(mutex)
        V(t1)
        
P(t2)
	room2 -= 1
     # critical section
    if room2 == 0:
        V(t1)
    else:
        V(t2)
```

#### 哲学家进餐问题 (Dining Philosophers)

```python
# Dining Philosophers hint #1
footman = Semaphore(4) # if only four philosophers are allowed at the table at a time

def get_forks(i):
    P(footman)
    P(fork[right(i)])
    P(fork[left(i)])
    
def put_forks(i):
    V(fork[right(i)])
    V(fork[left(i)])
    V(footman)
    
def philosopher():
    while True:
        think()
        get_forks()
        eat()
        put_forks()
        
Cobegin
	for i in range(5):
        philosopher()
Coend
```

```python
# Dining Philosophers hint #2
# deadlock is impossible if there's at least one leftie and at least one rightie at the table
```

```python
# Dining Philosophers hint #3
# Tanenbaum's solution, could cause starvation
state = ['thinking'] * 5 # the state of the five philosophers
sem = [Semaphore(0) for i in range(5)]
mutex = Semaphore(1)

def get_fork(i):
    P(mutex)
    state[i] = "hungry"
    test(i)
    V(mutex)
    P(sem[i])
    
def put_fork(i):
    P(mutex)
    state[i] = "thinking"
    test(right(i))
    test(left(i))
    V(mutex)
    
def test(i): # can start eating if i is hungry and neither of his neighbors are eating
    if state[i] == "hungry" and state[left(i)] != "eating" and state[right(i)] != "eating":
        state[i] = "eating"
        V(sem[i])
```

#### Cigarette smokers problem

```python
# Agent semaphores
agentSem = Semaphore(1)
isTobacco = isPaper = isMatch = False
tobaccoSem = Semaphore(0)
paperSem = Semaphore(0)
matchSem = Semaphore(0)
mutex = Semaphore(1)

# only some of the code is shown below
def PusherA():
    P(agentSem)
    P(tobacco)
    P(mutex)
    	if isPaper:
            isPaper = False
            V(matchSem)
        elif isMatch:
            isMatch = False
            V(paperSem)
        else:
            isTobacco = True
    V(mutex)
    
def smokerWithTobacco():
    P(tobaccoSem)
    makeCigarette()
    V(agentSem)
```

#### The Dining Savages Problem

> A tribe of savages eats communal dinners from a large pot that can hold M servings of stewed missionary1. When a savage wants to eat, he helps himself from the pot, unless it is empty. If the pot is empty, the savage wakes up the cook and then waits until the cook has refilled the pot.

```python
mutex = Semaphore(1) # protect the counter 'servings'
servings = 0 # number of servings
empty = Semaphore(1)
full = Semaphore(0)

def savages():
    while True:
	    P(mutex)
    	if servings == 0: # no servings in pot
        	P(full)
	        servings = M
    	    V(empty)
	    # still servings in pot
	    servings--
    	getServingsFromPot()
	    V(mutex)

def cook():
    while True:
	    P(empty)
	    putServingsInPot(M)
    	V(full)
        
Cobegin
	cook()
    for i in range(numOfSavages):
        savages()
Coend
```

#### The Barbershop Problem

> A barbershop consists of a waiting room with n chairs, and the barber room containing the barber chair. If there are no customers to be served, the barber goes to sleep. If a customer enters the barbershop and all chairs are occupied, then the customer leaves the shop. If the barber is busy, but chairs are available, then the customer sits in one of the free chairs. If the barber is asleep, the customer wakes up the barber. Write a program to coordinate the barber and the customers.

```python
mutex = Semaphore(1)
customerNum = 0 # the number of consumer in the shop
customer = Semaphore(0)
barber = Semaphore(0)
custermerDone = Semaphore(0)
barberDone = Semaphore(1)

def consumer():
    P(mutex)
    if customerNum == n:
        V(mutex)
        leaveTheShop() # leave the shop
        return
    customer += 1
    V(mutex)
    
    V(customer)
    P(barber)
    getHairCut() # get hair cut
    V(consumerDone)
    P(barberDone)
    
    P(mutex)
    customerNum -= 1
    V(mutex)
    
def barber():
    P(customer)
    V(barber)
    cutHair() # cut hair
    P(customerDone)
    V(barberDone)
```

#### The FIFO Barbershop Problem

```python
# serve the customer base on FIFO rule
customerNum = 0
mutex = Semaphore(1)
customer = Semaphore(0)
customerDone = Semaphore(0)
barberDone = Semaphore(0)
queue = []

def consumer():
    P(mutex)
    if customerNum == n:
        V(mutex)
        leaveTheShop() # leave the shop
        return
    customer += 1
    queue.append(self.sem) # append the Semaphore of the consumer himself
    V(mutex)
    
    V(customer)
    P(self.sem)
    getHairCut() # get hair cut
    V(consumerDone)
    P(barberDone)
    
    P(mutex)
    customerNum -= 1
    V(mutex)
    
def barber():
    P(customer)
    P(mutex)
    sem = queue.pop(0)
    V(mutex)
    
    V(sem)
    
    V(barber)
    cutHair() # cut hair
    P(customerDone)
    V(barberDone)
```

#### Hilzer's Barbershop Problem

> Our barbershop has three chairs, three barbers, and a waiting area that can accommodate four customers on a sofa and that has standing room for additional customers. Fire codes limit the total number of customers in the shop to 20.
>
> A customer will not enter the shop if it is filled to capacity with other customers. Once inside, the customer takes a seat on the sofa or stands if the sofa is filled. When a barber is free, the customer that has been on the sofa the longest is served and, if there are any standing customers, the one that has been in the shop the longest takes a seat on the sofa. When a customer’s haircut is finished, any barber can accept payment, but because there is only one cash register, payment is accepted for one customer at a time. The barbers divide their time among cutting hair, accepting payment, and sleeping in their chair waiting for a customer.

#### The Santa Claus Problem

> Stand Claus sleeps in his shop at the North Pole and can only be awakened by either (1) all nine reindeer being back from their vacation in the South Pacific, or (2) some of the elves having difficulty making toys; to allow Santa to get some sleep, the elves can only wake him when three of them have problems. When three elves are having their problems solved, any other elves wishing to visit Santa must wait for those elves to return. If Santa wakes up to find three elves waiting at his shop’s door, along with the last reindeer having come back from the tropics, Santa has decided that the elves can wait until after Christmas, because it is more important to get his sleigh ready. (It is assumed that the reindeer do not want to leave the tropics, and therefore they stay there until the last possible moment.) The last reindeer to arrive must get Santa while the others wait in a warming hut before being harnessed to the sleigh.
>
> Here are some addition specifications:
>
> • After the ninth reindeer arrives, Santa must invoke `prepareSleigh`, and then all nine reindeer must invoke `getHitched`. 
> • After the third elf arrives, Santa must invoke `helpElves`. Concurrently, all three elves should invoke `getHelp`. 
> • All three elves must invoke `getHelp` before any additional elves enter (increment the elf counter).
>
> Santa should run in a loop so he can help many sets of elves. We can assume that there are exactly 9 reindeer, but there may be any number of elves.

```python
elves = 0 # counter
reindeer = 0 # counter
santaSem = Semaphore(0) # santa waits on santaSem until the elves or reindeer wakes him
reindeerSem = Semaphore(0) # the reindeer wait on reindeerSem umtil Santa signals them to get hitched
elfTex = Semaphore(1) # the elves ise elfTex to prevent additional elves from entering while three elves are being helped
mutex = Semaphore(1) # protect elves & reindeer

def santa():
    P(santaSem)
    P(mutex)
    	if reindeer >= 9:
            reindeer -= 9
            prepareSleigh()
            for i in range(9): # reindeerSem.signal(9)
                V(reindeerSem)
         elif elvs == 3: # elves needs help
            helpElves()
     V(mutex)
            
def reindeer():
    P(mutex)
    reindeer += 1
    if reindeer == 9:
        V(santaSem)
    V(mutex)
    
    P(reindeerSem)
    getHitched()
    
def elves():
    P(elfTex)
    P(mutex)
    	elves += 1
    	if elves == 3:
            V(santaSem)
        else:
            V(elfTex)
    V(mutex)
    
    getHelp()
    
    P(mutex)
    	elves -= 1
        if elves == 0:
            V(elfTex)
    V(mutex)
```

#### Building H2O

> There are two kinds of threads, oxygen and hydrogen. In order to assemble these threads into water molecules, we have to create a barrier that makes each thread wait until a complete molecule is ready to proceed. As each thread passes the barrier, it should invoke bond. You must guarantee that all the threads from one molecule invoke bond before any of the threads from the next molecule do.
> In other words:
> • If an oxygen thread arrives at the barrier when no hydrogen threads are present, it has to wait for two hydrogen threads.
> • If a hydrogen thread arrives at the barrier when no other threads are present, it has to wait for an oxygen thread and another hydrogen thread.

```python
Ocount = 0
Hcount = 0
mutex = Semaphore(1)
OCanBond = Semaphore(0)
HCanBond = Semaphore(0)

def Oxygen():
    P(mutex)
    	Ocount += 1
        if Hcount >= 2:
            Hcount -= 2
            bond()
            HCanBond.signal(2)
            V(mutex)
        else:
            V(mutex)
            P(OCanBond)
            bond()
    
def Hydrogen():
    P(mutex)
    	Hcount += 1
        if Ocount >= 1 and Hcount >= 2:
            bond()
            V(OCanBond)
            V(HCanBond)
            V(mutex)
        else:
            V(mutex)
            P(HCanBound)
            bond()
```

#### River Crossing Problem

> Somewhere near Redmond, Washington there is a rowboat that is used by both Linux hackers and Microsoft employees (serfs) to cross a river. The ferry can hold exactly four people; it won’t leave the shore with more or fewer. To guarantee the safety of the passengers, it is not permissible to put one hacker in the boat with three serfs, or to put one serf with three hackers. Any other combination is safe.
> As each thread boards the boat it should invoke a function called `board`. You must guarantee that all four threads from each boatload invoke board before any of the threads from the next boatload do. After all four threads have invoked board, exactly one of them should call a function named `rowBoat`, indicating that that thread will take the oars. It doesn’t matter which thread calls the function, as long as one does.

```python
barrier = Barrier(4)
mutex = Semaphore(1)
hackers = 0 # the number of hackers waiting to board
serfs = 0 # the number of servers waiting to board
hackerQueue = Semaphore(0)
serfQueue = Semaphore(0)
local isCaptain = False

def hacker():
    P(mutex)
	hackers += 1
    if hackers >= 4:
        board()
        hackerQueue.signal(3)
        rowBoat()
        V(mutex)
    elif hackers >= 2 and serfs >= 2:
        board()
        hackerQueue.signal(1)
        serfQueue.signal(2)
        rowBoat()
        V(mutex)
    else:
        V(mutex)
        P(hackerQueue)

def serf():
    P(mutex)
    serfs += 1
    if serfs >= 4:
        board()
        serfQueue.signal(3)
        rowBoat()
        V(mutex)
    elif hackers >= 2 and serf >= 2:
        board()
        hackerQueue.signal(2)
        serfQueue.signal(1)
        rowBoat()
        V(mutex)
    else:
        V(mutex)
        P(serfQueue)
```

#### The Roller Coaster Problem

> Suppose there are n passenger threads and a car thread. The passengers repeatedly wait to take rides in the car, which can hold C passengers, where C < n. The car can go around the tracks only when it is full.
> Here are some additional details:
> • Passengers should invoke `board` and `unboard`. 
> • The car should invoke `load`, `run` and `unload`. 
> • Passengers cannot `board` until the car has invoked `load`
> • The car cannot `depart` until C passengers have `boarded`.
> • Passengers cannot `unboard` until the car has invoked `unload`.

```python
mutex = Semaphore (1)
mutex2 = Semaphore (1)
boarders = 0
unboarders = 0
boardQueue = Semaphore (0)
unboardQueue = Semaphore (0)
allAboard = Semaphore (0)
allAshore = Semaphore (0)

def car():
    load()
    boardQueue.signal(C)
    allAboard.wait()
    
    run()
    
    unload()
    unboardQueue.signal(C)
    allAshore.wait()
    
def passenger():
    boardQueue.wait()
    board()
    
    mutex.wait()
    	boarders += 1
        if boarders == C:
            allAboard.signal()
            boarders = 0
    mutex.signal()
    
    unboardQueue.wait()
    unboard()
    
    mutex2.wait()
    	unboarders += 1
        if unboarders == C:
            allAshore.signal()
            unboarders = 0
    mutex2.signal()
```

#### The Search-Insert-Delete Problem

> Three kinds of threads share access to a singly-linked list: searchers, inserters and deleters. Searchers merely examine the list; hence they can execute concurrently with each other. Inserters add new items to the end of the list; insertions must be mutually exclusive to preclude two inserters from inserting new items at about the same time. However, one insert can proceed in parallel with any number of searches. Finally, deleters remove items from anywhere in the list. At most one deleter process can access the list at a time, and deletion must also be mutually exclusive with searches and insertions.

```python
searcher = 0 # number of searchers
inserter = 0 # number of inserters
searcherMutex = semaphore(1)
inserterMutex = Semaphore(1)
noSearcher = Semaphore(1)
noInserter = Semaphore(1)

def search():
    P(searcherMutex)
	    searcher += 1
        if searcher == 1: # the first searcher
            P(noSearcher)
    V(searcherMutex)
    # critical section
    P(searcherMutex)
    	searcher -= 1
        if searcher == 0: # the last searcher
            V(noSearcher)
    V(searcherMutex)
    
def insert():
    P(noInserter)
    P(inserterMutex)
    # critical section
    V(inserterMutex)
    V(noInserter)
    
def delete():
    P(noSearcher)
    P(noInserter)
    # critical section
    V(noInserter)
    V(noSearcher)
```

#### The Unisex Bathroom Problem

> • There cannot be men and women in the bathroom at the same time.
>
> • There should never be more than three employees squandering company time in the bathroom.

#### Baboon Crossing Problem

#### The Modus Hall Problem

> After a particularly heavy snowfall this winter, the denizens of Modus Hall created a trench-like path between their cardboard shantytown and the rest of campus. Every day some of the residents walk to and from class, food and civilization via the path; we will ignore the indolent students who chose daily to drive to Tier 3. We will also ignore the direction in which pedestrians are traveling. For some unknown reason, students living in West Hall would occasionally find it necessary to venture to the Mods.
> Unfortunately, the path is not wide enough to allow two people to walk side-by-side. If two Mods persons meet at some point on the path, one will gladly step aside into the neck high drift to accommodate the other. A similar situation will occur if two ResHall inhabitants cross paths. If a Mods heathen and a ResHall prude meet, however, a violent skirmish will ensue with the victors determined solely by strength of numbers; that is, the faction with the larger population will force the other to wait.

#### The Sushi Bar Problem

>  Imagine a sushi bar with 5 seats. If you arrive while there is an empty seat, you can take a seat immediately. But if you arrive when all 5 seats are full, that means that all of them are dining together, and you will have to wait for the entire party to leave before you sit down.

```c
seat = 0;
mutex = Semaphore(1);
empty = Semaphore(1);

consumer() {
    P(mutex);
    if (seat < 5) {
        seat++;
    	if (seat == 5) {
        	P(empty);
            V(mutex);
    } else { // all seats are taken
        P(empty);
        seat++;
        V(mutex);
    }
    eat();
    P(mutex);
    seat--;
    if (seat == 0) {
        for (int i = 0; i++; i < 5) {
            V(empty);
        }
    }
    V(mutex);
}
```

```python
eating = waiting = 0 # the number of threads sitting at the bar and waiting
mutex = Semaphore(1) # protects eating and waiting and must_wait
block = Semaphore(0)
must_wait = False # indicates that the bar is (has been) full

# first possible solution
def consumer1():
    P(mutex)
    if must_wait:
        waiting += 1
        V(mutex)
        P(block)
    else:
        eating += 1
	    must_wait = (eating == 5)
    	V(mutex)
    
    # eat sushi
    
    P(mutex)
    eating -= 1
    if eating == 0:
        n = min(5, waiting)
        waiting -= n
        eating += n
        must_wait = (eating == 5)
        for i in range(n):
            V(block)
    V(mutex)
    
# another possible solution
def consumer2():
    P(mutex)
    if must_wait:
        waiting += 1
        V(mutex)
        P(block)
        wating -= 1
    
    eating += 1
    must_wait = (eating == 5)
    if waiting and not must_wait:
        V(block)
    else:
        V(mutex)
        
    # eat sushi
    
    V(mutex)
    eating -= 1
    if eating == 0:
        must_wait = False
    if waiting and not must_wait:
        V(block) # and pass the mutex
    else:
        V(mutex)
```

#### The Child Care Problem

> At a child care center, state regulations require that there is always one adult present for every three children.

```python
multiplex = Semaphore(0) # counts the number of tokens available
mutex = Semaphore(1)

def adult():
    multiplex.signal(3)
    # critical section
    P(mutex)
	    P(multiplex)
    	P(multiplex)
	    P(multiplex)
    V(mutex)
```

#### The Room Party Problem

> The following synchronization constraints apply to students and the Dean of Students:
>
> 1. Any number of students can be in a room at the same time.
>
> 2. The Dean of Students can only enter a room if there are no students in the room (to conduct a search) or if there are more than 50 students in the room (to break up the party).
>
> 3. While the Dean of Students is in the room, no additional students may enter, but students may leave.
>
> 4.  The Dean of Students may not leave the room until all students have left.
>
> 5.  There is only one Dean of Students, so you do not have to enforce exclusion among multiple deans.

```python
# meaningless
student = 0
status = "waiting" # status of the dean
mutex = Semaphore(1)
dean = Semaphore(0)
noDean = Semaphore(1)
leave = Semaphore(0)

def dean():
    status = "waiting"
    P(dean)
    status = "inside"
    # enter the room
    P(leave)
    # leave the room
    V(noDean)
    status = "waiting"
    
def student():
    P(noDean) # if dean is in the room, the student cannot enter
    P(mutex)
    student += 1
    if student == 1:
        P(dean) # dean cannot enter the room if students (1 <= x <= 50) in the room
    elif student == 51: # more than 50 student in room
        V(dean) # dean can enter
    V(mutex)
    # student hang out
    P(mutex)
    student -= 1
    if student == 0:
        V(leave)
        V(dean) # dean can enter the room if it is empty
    elif student <= 50:
        P(dean) # dean cannot enter the room if it has less than 50 people
    V(mutex)
```

```python
students = 0 # number of students in the room
dean = "not here" # the status of the dean
mutex = Semaphore(1) # protect student and dean
turn = Semaphore(1) # turnstile that keeps students from entering while the Dean is in the room
clear = Semaphore(0) 
lieIn = Semaphore(0) # clean and lieIn are used as rendezvouses between a student and the Dean

def Dean():
    P(mutex)
    	if students > 0 and students < 50:
            dean = "waiting"
            P(lieIn) # get mutex from the student
        # students must be 0 or >= 50
        if students >= 50:
            dean = "in the room"
            breakup() # break up the party
            P(turn) # lock the turnstile
            V(mutex)
            P(clear) # get mutex from the student
            V(turn) # unlock the turnstile
        else: # student === 0
            search()
    dean = "not here"
    V(mutex)
    
def student():
    P(mutex)
    	if dean == "in the room":
            V(mutex) # cannot enter the room if dean is there
            P(turn)
            V(turn)
            P(mutex)
        students += 1
        if students == 50 and dean == "waiting":
            V(lieIn) # pass mutex to the dean
        else:
            V(mutex)
    party()
    P(mutex)
    	students -= 1
        if students == 0 and dean == "waiting":
            V(lieIn) # pass mutex to the dean
        elif students == 0 and dean == "in the room":
            V(clear) # pass mutex to the dean
        else:
            V(mutex)
```

#### The Senate Bus Problem

> Riders come to a bus stop and wait for a bus. When the bus arrives, all the waiting riders invoke `boardBus`, but anyone who arrives while the bus is boarding has to wait for the next bus. The capacity of the bus is 50 people; if there are more than 50 people waiting, some will have to wait for the next bus.
> When all the waiting riders have boarded, the bus can invoke `depart`. If the bus arrives when there are no riders, it should depart immediately.

```python
# error
waitingQueue = 0
mutex = Semaphore(1)
getIn = Semaphore(1)
arrive = Semaphore(1)

def bus():
    P(arrive)
    P(mutex)
    	if waitingQueue >= 0: # nobody is waiting
            n = min(50, waitingQueue)
            for i in range(n):
                V(getIn)
        depart()
    V(mutex)
    V(arrive)
    
def passenger():
    P(mutex)
    	waitingQueue += 1
    V(mutex)
    P(arrive)
    V(arrive)
    P(getIn)
    boardBus()
```

```python
# solution 1
riders = 0 # how many riders are waiting
mutex = Semaphore(1) # protects riders
multiplex = Semaphore(50) # make sure there are no more than 50 riders in the boarding area
bus = Semaphore(0) # riders wait on bus
allAboard = Semaphore(0) # bus wait on allAboard

def bus():
    P(mutex)
    if riders > 0:
        V(bus) # pass the mutex
        P(allAboard)
    V(mutex)
    depart()
    
def riders():
    P(multiplex)
    P(mutex) # cannot enter if the bus arrives
    	riders += 1
    V(mutex)
    P(bus) # get the mutex
    V(multiplex)
    boardBus()
    P(mutex)
    riders -= 1
    if riders == 0:
        V(allAboard)
    else:
        V(bus) # pass the mutex
    V(mutex)
```

```python
# solution 2
waiting = 0 # the number of riders in the boarding area
mutex = Semaphore(1) # protect waiting
bus = Semaphore(0) # signals when the bus has arrived
boarded = Semaphore(0) # signals that a rider has boarded

def bus():
    P(mutex)
    n = min(waiting, 50)
    for i in range(n):
        V(bus)
        P(boarded)
    waiting = max(waiting-50, 0)
    V(mutex)
    depart()
    
def rider():
    P(mutex)
    waiting += 1
    V(mutex)
    P(bus)
    boardBus()
    V(boarded)
```

#### The Faneuil Hall Problem

> There are three kinds of threads: immigrants, spectators, and a one judge. Immigrants must wait in line, check in, and then sit down. At some point, the judge enters the building. When the judge is in the building, no one may enter, and the immigrants may not leave. Spectators may leave. Once all immigrants check in, the judge can confirm the naturalization. After the confirmation, the immigrants pick up their certificates of U.S. Citizenship. The judge leaves at some point after the confirmation. Spectators may now enter as before. After immigrants get their certificates, they may leave.
>
> To make these requirements more specific, let’s give the threads some functions to execute, and put constraints on those functions.
> • Immigrants must invoke `enter`, `checkIn`, `sitDown`, `swear`, `getCertificate` and `leave`. 
> • The judge invokes `enter`, `confirm` and `leave`. 
> • Spectators invoke `enter`, `spectate` and `leave`. 
> • While the judge is in the building, no one may enter and immigrants may not leave. 
> • The judge can not confirm until all immigrants who have invoked enter have also invoked `checkIn`. 
> • Immigrants can not `getCertificate` until the judge has executed `confirm`.

#### Dining Hall Problem

> Students in the dining hall invoke `dine` and then `leave`. After invoking `dine` and before invoking `leave` a student is considered “ready to leave”.
> The synchronization constraint that applies to students is that, in order to maintain the illusion of social suave, a student **may never sit at a table alone.** A student is considered to be sitting alone if everyone else who has invoked dine invokes leave before she has finished dine.

```python
eating = 0
readyToLeave = 0
mutex = Semaphore(1)
okToLeave = Semaphore(0)

def student():
    getFood()
    P(mutex)
    	eating += 1
        if eating == 2 and readyToleave == 1:
            V(okToLeave)
            readyToLeave -= 1
    V(mutex)
    dine()
    P(mutex)
    eating -= 1
    readyToLeave += 1
    if eating == 1 and readyToLeave == 1:
        V(mutex)
        P(okToLeave)
    elif eating == 0 and readyToLeave == 2:
        V(okToLeave)
        readyToLeave -= 2
        V(mutex)
    else:
        readyToLeave -= 1
        V(mutex)
    leave()
```



### 基本概念

#### 调度

CPU 调度的任务是控制、协调多个进程对 CPU 的竞争。也就是按照一定的策略，从就绪队列中选择一个进程，并把 CPU 控制权交给被选中的进程。

### 要考虑的问题

* **WHAT**: 按照什么原则选择下一个要执行的进程——进程调度算法
* **WHEN**: 何时分配 CPU ——进程调度时机
* **HOW**: 如何分配 CPU ——CPU 切换过程

### ==调度类型==

#### 高级调度

> 又称为 **宏观调度**，**作业调度**。从**用户工作流程**的角度，一次提交的如若干个作业，对每个作业进行调度。

#### 中级调度

> 又称为 **内外存切换**。从**存储器资源**的角度，将进程的部分或全部换出到外存上，将当前所需部分换入内存。指令和数据必须在内存里才能被 CPU 直接访问。

#### 低级调度

> 又称为 **微观调度**，**进程或线程调度**。从 **CPU 资源**的角度，执行的单位，时间上通常为毫秒。

* 非抢占式
* 抢占式
  * 时间片原则
  * 优先权原则
  * 短作业（进程）优先

### CPU 三级调度

![image](https://img2020.cnblogs.com/blog/2324053/202105/2324053-20210531220906682-1320796306.png)

### ==调度的性能准则==

#### 周转时间——批处理系统

作业从提交到完成（得到结果）所经历的时间。

包括：

* 在收容队列里等待
* 在 CPU 上执行
* 在就绪队列和阻塞队列中等待
* 结果输出等待

##### 平均周转时间

##### 带权周转时间

##### 外存等待时间

##### 就绪等待时间

##### CPU 执行时间

##### I/O 操作时间

#### 响应时间——分时系统

用户输入一个请求到系统首次做出响应的时间

#### 截止时间——实时系统

开始截止时间和完成截止时间

#### 优先级

可以使关键任务达到更好的指标

#### 公平性

不因作业或进程本身的特性而使上述指标过分恶化，如长作业等待很长时间等

#### 吞吐量——批处理系统

单位时间内所完成的作业数，跟作业本身特性和调度算法都有关系

#### 处理机利用率——大中型主机

#### 各种资源的均衡利用

### 设计调度算法要点

#### 进程优先级

* 静态优先级

  进程创建时指定，运行过程中不再改变

* 动态优先级

  进程创建时指定，运行过程中可以动态变化

#### 进程优先级就绪队列的组织

* 按优先级排队
* 所有进程创建以后进入第一优先级就绪队列，随着进程的运行，可能会降低某些进程的优先级，如某些进程的时间片用完了，就将其降级

#### 抢占式与非抢占式调度

##### 不可抢占式

一旦处理器分配给某一个进程，它就一直占用处理器，直到该进程自己因调用原语操作或等待 I/O 等原因进入阻塞状态，或等时间片用完才让出处理器

##### 抢占式

就绪队列中一旦有优先级高于当前运行进程优先级的进程存在时，便立即进行进程调度，把处理器转给优先级高的进程

#### 进程的分类

* I/O 密集型
* CPU 密集型
* **批处理进程**
  * 无需与用户交互，通常在后台运行
  * 无需很快响应
* 交互式进程
  * 与用户交互频繁
  * 响应时间快
* 实时进程
  * 有实时需求
  * 响应时间短且稳定

#### 时间片

确定了允许该进程运行的时间长度

### 批处理系统的调度算法

#### ==公式==

$$
吞吐量 = \frac{作业数}{总执行时间} \\
周转时间 = 完成时刻 - 提交时刻 \\
带权周转时间 = \frac{周转时间}{服务时间（执行时间）} \\
平均周转时间 = \frac{作业周转时间之和}{作业数} \\
平均带权周转时间 = \frac{作业带权周转时间之和}{作业数}
$$

#### ==常见调度算法==

##### 先来先服务 (FCFS: First Come First Serve)

* 规则：
  * 按照作业提交或进程变为就绪状态的先后顺序分派 CPU
  * 非抢占
* 利弊：
  * 有利于长作业，不利于短作业
  * 有利于 CPU 繁忙的作业，不利于 I/O 繁忙的作业

##### 最短作业优先 (SJF: Shortest Job First)

* 规则：
  * 对预计执行时间短的作业优先分派处理机
  * 通常非抢占
* 利弊：
  * 相比于 FCFS 减少了平均周转时间
  * 对于长作业不利
  * 未能根据作业紧迫程度划分执行优先级
  * 难以准确估计作业（进程）优先级，从而影响调度性能

##### 最短剩余时间优先 (FRTF: Shortest Remaining Time First)

* 规则
  * 对预计剩余时间短的作业优先分派处理机
  * **抢占式**
* 利弊
  * 源源不断的短任务可能使长任务得不到运行，从而产生”饥饿“现象

##### 最高响应比优先 (HRRF: Response Ratio First)

* 规则

  * 每次选择作业投入运行时，先计算后备作业队列中每个作业的响应比 RP，然后选择其值最大的作业投入运行

    > $RP = \frac{已等待时间 + 要求运行时间}{要求运行时间}$

* 利弊

  * 每次计算各道作业的响应比会有一定的时间开销，性能略差于 SJF

### 交互式系统的调度算法

#### 时间片轮转 (RR: Round Robin)

* 规则
  1. 将系统中所有就绪进程按照 FCFS 原则排成一个队列
  2. 每次调度时将 CPU 分派给队首进程，让其执行一个时间片
  3. 在一个时间片结束时，发生时钟中断
  4. 调度程序暂停当前进程，将其送到就绪队列的末尾
  5. 通过上下文切换执行当前的队首进程
* 时间片长度的确定
* 利弊

#### 优先级算法 (Priority Scheduling)

##### 静态优先级

##### 动态优先级

#### 多级队列 (MQ: Multi-level Queue)

* 规则
  * 引入多个就绪队列
  * 不同队列可有不同优先级、时间片长度、调度策略
  * 在运行过程中可以改变进程所在队列
* 利弊
  * 提高系统吞吐量，缩短平均周转时间，照顾短进程
  * 获得较好的 I/O 设备利用率和缩短响应时间而照顾 I/O 型进程
  * 不必估计进程的执行时间，动态调节

#### 多级反馈队列 (MFQ: Multi-level Feedback Queue)

* 规则
  * 设置多个就绪队列，分别赋予不同优先级（队列 1 优先级最高，优先级越低时间片越长）
  * 新进程进入内存后，先投入队列 1 末尾，按照 FCFS 算法调度
  * 若在队列 1 中未执行完，则投入到队列 2 末尾
  * 降低到最后的队列中时，按照“时间片轮转”调度算法直到完成

### 优先级倒置

高优先级进程（或线程）被低优先级进程（或线程）延迟或阻塞

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210601093327005-1377194481.png)

* 解决方法——优先级置顶 (Priority Ceiling)

  ![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210601093403148-1682096032.png)

* 解决方法——优先级继承 (Priority Inheritance)

  ![image-20210601093454293](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210601093454293.png)

### 实时系统的调度算法

> 实时系统是一种 **时间** 起着主导作用的系统。计算机必须在一个确定的时间范围内恰当的对外界刺激做出反应。
>
> * 硬实时系统
>
>   绝对满足截止时间要求
>
> * 软实时系统
>
>   可以偶尔不满足截至时间要求

##### 静态表调度 (Static Table-driven Scheduling)

通过对所有周期性任务的分析预测实现确定一个固定的调度方案

* 特点
  * 无任何计算，按固定方案进行，开销最小
  * 无灵活性，只适用于完全固定的任务场景

##### 单调速率调度 (RMS: Rate Monotonic Scheduling)

*单处理器下的**最优静态**调度算法*

* 特点
  * 任务周期越小，其优先级越高，优先级最高的任务优先被调度
  * 如果两个任务优先级一样，将随机选择一个调度
  * **静态，抢占式调度**

##### 最早截至时间优先算法 (EDF: Earliest Deadline First)

* 特点
  * 任务的绝对截止时间越早，其优先级越高，优先级最高的任务优先被调度
  * 如果两个任务优先级一样，将随机选择一个调度

### 多处理器调度

* 与单处理器调度的区别
  * 注重整体运行效率
  * 更多样的调度算法
  * 广泛采用多线程调度

## 死锁

### 死锁的概念

#### 死锁问题 (Deadlock)

* 定义
  * 由于资源占用的互斥，当某个进程提出资源申请之后，使得一些进程在无外力协助的情况下，永远分配不到必需的资源而无法运行
* 发生原因
  * **竞争资源**
  * **并发执行的顺序不当**

#### ==死锁发生的四个必要条件==

1. **互斥条件**

   指进程对所分配到的资源进行排他性使用，即在一段时间内资源只能由一个进程占用。

2. **请求和保持条件**

   指进程已经保持至少一个资源，但又提出新的资源请求，而该资源已被其它进程占有，此时请求进程阻塞，但又对自己已获得的其它资源保持不放

3. **不剥夺条件**

   指进程已获得的资源，在未使用完之前，不能被剥夺，只能在使用完成时由自己释放

4. **环路等待条件**

   指在死锁发生时，必然存在一个进程——资源的环形链

#### 活锁 (Livelock)

指的是任务或执行者没有被阻塞，或者由于某些条件没有满足，导致一直重复尝试和失败

#### 饥饿 (Starvation)

某些进程可能由于资源分配不公平导致长时间等待。当等待时间给进程推进和响应带来明显影响时，称发生了进程饥饿。当饥饿到一定程度的进程所赋予的任务即使完成也不再具有实际意义时称为进程被 **饿死**

### 处理死锁的基本方法

* 不允许死锁发生

  * 预防死锁（静态）

    破坏死锁的产生条件

  * 避免死锁（动态）

    在资源分配之前进行判断

* 允许死锁发生

  * 检测和解除死锁

  * 无所作为

    鸵鸟算法

#### 死锁预防【静态】

1. **打破互斥条件**

   允许进程同时访问某些资源

2. **打破占有且申请条件**

   实行资源预先分配策略。只有当系统能够满足当前进程的全部资源需求时，才一次性的将所申请的资源全部分配给该进程，否则不分配任何资源。

   * 不可预测性
   * 资源利用率低
   * 降低进程的并发性

3. **打破不可剥夺条件**

   允许进程强行从占有者那里夺取资源。

   实现困难，会降低系统性能

4. **打破循环等待条件**

   实现资源有序分配策略，即把资源事先分类编号，按号分配，使得进程在申请时，占用资源不会形成环路。所有进程对资源的请求必须严格按照资源序号递增的顺序提出。进程只有占用了小号资源，才能申请大号资源。

   增加系统开销，增加进程对资源的占用时间

#### ==死锁避免【动态】==

##### 安全序列

* 系统中的所有进程能够按照某种次序分配资源，并依序执行完毕，这种进程序列就称为 **安全序列**。
* 如果 **存在** 一个安全序列，则系统是 **安全** 的。若 **不存在** 安全序列，则系统是不安全的。

##### 安全状态

* **安全状态**： 系统存在一个进程执行序列 <P~1~, P~2~, ... P~n~> 能顺利完成
* **不安全状态**：系统不存在可完成的序列
* *系统进入不安全状态也未必会产生死锁，但产生死锁后，系统一定处于不安全状态*

##### ==银行家算法==

###### 数据说明

* $n$ 为进程数量，$m$ 为资源类型数量

* 可利用资源向量 $Avaliable$: $m$ 维向量

  具有 $m$ 个元素的向量，其中每一个元素代表一类可利用的资源数目，其初值是系统中所配置的该类全部可用的资源数目。

* 最大需求矩阵 $Max$: $n \times m$ 矩阵

  定义了系统中 $n$ 个进程中的每一个进程对 $m$ 类资源的最大需求

* 分配矩阵 $Allocation$: $n\times m$ 矩阵

  定义了系统中每一类资源当前已分配给每一进程的资源数

* 需求矩阵 $Need$: $n\times m$ 矩阵

  表示每一个进程尚需的各类资源数

  $Need(i, j) = Max(i, j) - Allocation(i, j)$

###### 具体算法

> 设 $Request$ 是进程 $P_i$ 的请求向量，此时进程 $P_i$ 需要 $k$ 个 $R_j$ 类资源

1. 若 $Request_i \le Need_i$，则转向步骤 2，否则出错（所需要的资源超过宣布的最大值）

2. 若 $Request_i \le Avaliable_i$，则转向步骤 3，否则，表示系统尚无足够的资源，$P_i$ 需等待

3. 系统 **试探** 地把资源分配给进程 $P_i$
   $$
   Avaliable := Avaliable - Request \\
   Allocation := Allocation + Request \\
   Need_i := Need_i - Request
   $$

4. 系统执行 **安全性算法**，检查此次资源分配后，系统是否处于安全状态

   * 若安全，则正式将资源分配给进程 $P_i$
   * 若不安全，则恢复原理啊的进程分配状态，让 $P_i$ 等待

   > 安全性算法：是否有安全序列

#### 检测死锁

##### 资源分配图 (RAG) 算法

> 资源分配图 RAG(Resource Allocation Graph)
>
> 有向图 $G$ 的顶点为 **资源R** 或 **进程P**.
>
> * $R\rightarrow P$: $R$ 已分配给 $P$
> * $P\rightarrow R$: $P$ 正因请求 $R$ 而处于等待状态
>
> 化简：
>
> ![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210601205645257-873812331.png)

**死锁定理**：系统中某个时刻 $t$ 为死锁状态 $\Leftrightarrow$ $t$ 时刻系统的资源分配图是 **不可能完全化简的**

#### 死锁解除

##### 资源剥夺法

使用挂起 / 激活挂起一些进程，剥夺它们的资源解除死锁，当条件满足时，再激活进程

##### 撤销进程法

将全部死锁的进程夭折掉，按照某个顺序逐个撤销（回退）进程，直至有足够的资源可用，死锁状态解除为止

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210601210214512-866551624.png)

### 死锁的基本问题

#### 哲学家就餐问题

> **解题思路**
>
> 1. 至多只允许 *四个* 哲学家（尝试）进餐【破除资源互斥】
> 2. 对筷子进行 *编号*，每个哲学家按照编号从低到高拿筷子。或对哲学家 *编号*，奇数号哲学家先拿左边筷子，偶数号先拿右边筷子【破除循环等待】
> 3. 同时拿起两根筷子，否则不拿起【破除保持等待】

```c
semaphore fork[5] = {1}; // 筷子数量
semaphore room = {4}; // 桌子周围的空间，最多能容纳四个哲学家
int i;
void philosophoer (int i)
{
    while (true) {
        think();
        P(room); // 最多只允许四个哲学家同时坐在桌子周围
        P(fork[i]);
        P(fork[(i+1) mod 5]);
        eat(); // 进餐
        V(fork[(i + 1) mod 5]);
        V(fork[i]);
        V(room); // 释放资源
    }
}

void main()
{
    parbegin (philosopher(0), philosopher(1), philosopher(2), philosopher(3), philosopher(4));
}
```

## 设备管理

### I/O 管理概述

#### 设备管理的目的和功能

* 外设管理目的
  * 提高效率
  * 方便使用
  * 方便控制
* 外设管理功能
  * 提供设备使用的用户接口
  * 设备分配和释放
  * 设备的访问控制
  * I/O 缓冲和调度

#### 总线(bus)：接入 I/O 设备的主要方式

#### I/O 管理的特点

I/O 性能经常成为系统性能的瓶颈

#### I/O 管理的目标和任务

1. 按照用户请求，控制设备操作，完成 I/O 设备与内存间的数据交换，最终完成用户的 I/O 请求
2. 建立方便、统一的独立于设备的接口
3. 提高 I/O 效率
4. 保护数据安全性

#### I/O 设备的分类

##### 按数据组织分类

* **块设备**：

  以**数据块**为单位存储、传输信息。**传输速率较高**，**可寻址**（随机读写）

  > 硬盘、软盘驱动器、CD-ROM 驱动器、闪存、U 盘、SD 卡
  >
  > **随机读写**：意味着可以不按照顺序读写。

* **字符设备**：

  以**字符**为单位随机存储、传输信息。**传输速率低**，**不可寻址**
  
  > e.g. 鼠标、键盘、串口、控制台和 LED 设备
  
* **网络设备**

##### 按用途分类

* **存储设备**：磁盘、磁带
* **传输设备**：网卡、Modem
* **人机交互设备**：显示器、键盘、鼠标

**从资源分配角度**

* **独占设备**：在一段时间内只能由一个进程使用的设备，如打印机等
* **共享设备**：在一段时间内允许多个进程共同使用的设备，如硬盘
* **虚设备**：在一类设备上模拟另一类设备。*用共享设备模拟独占设备，用高速设备模拟低速设备。*

### I/O 硬件组成

#### 设备控制器

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607110550864-979622355.png)

* 功能
  * 接受 & 识别 CPU 命令
  * 数据交换：CPU 与控制器、控制器与设备
  * 设备状态的了解和报告
  * 设备地址识别
  * 缓冲区
  * 对设备传来的数据进行差错检测
* 组成
  * **控制器与 CPU 接口**：数据寄存器、状态寄存器
  * **控制器与设备接口**：数据信号、控制信号、状态信号
  * **I/O 逻辑**：用于实现 CPU 对 I/O 设备的控制

#### I/O 端口地址

* I/O 端口地址：接口电路中每个寄存器有唯一的地址

* 所有 I/O 端口地址形成 I/O 端口的 **地址空间**

  * I/O 独立编址

    > Intel 体系结构 in/out 指令

    * 优点
      * 外设不占用内存的地址空间
      * 编程时易于区分是对内存操作还是对 I/O 操作
    * 缺点
      * I/O端口操作的指令类型少，操作不灵活

  * 内存映像编址

    > 控制器的内存 / 寄存器作为物理内存空间的一部分

    * **优点**：
      * 不需要特殊的保护机制来阻止用户进程进行相应的 I/O 操作
      * 可以引用内存的每一条指令都适用于引用控制寄存器
    * **缺点**：
      * **不允许对一个控制寄存器的内容进行高速缓存**

### ==I/O 控制方式==

#### 程序控制 I/O (PIO, Programmed I/O)

> 也称 **轮询** 或 **查询方式 I/O**，由 CPU 代表进程向 I/O 模块发出指令，然后进入忙等状态，知道操作完成之后进程继续执行

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607112151271-579023237.png)

#### 中断驱动方式 (Interrupt-driven I/O)

> I/O 操作结束后由设备控制器主动通过中断通知设备驱动程序。

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607112217758-1752791991.png)

#### 直接存储访问方式 (DMA, Direct Memory Access)

> 由一个专门的控制器完成数据从内存到设备或设备到内存的传输工作

1. 由程序设置 DMA 控制器中的若干寄存器值（如内存始址，传送字节数），然后发起 I/O 操作
2. DMA 控制器完成内存与外设的成批数据交换
3. 在操作完成时由 DMA 控制器向 CPU 发出中断

**DMA 控制器中的寄存器**

* **命令 / 状态寄存器(CR)**，用于接收从 CPU 发来的 I/O 命令，或有关控制信息，或设备的状态
* **内存地址寄存器(MAR)**，在输入时，它存放把数据从设备传送到内存的起始目标地址，在输出时，它存放由内存到设备的内存源地址
* **数据寄存器(DR)**，用于暂存从设备到内存，或从内存到设备的数据
* **数据计数器(DC)**，存放本次 CPU 要读或写的字（节）数

**优缺点**

* 优点
  * CPU 只需干预 I/O 操作的开始和结束，而后续成批的数据读写则无需 CPU 控制，适用于高速设备
* 缺点
  * 数据传送的方向、存放数据的内存地址及传送数据的长度等都由 CPU 控制，占用了 CPU 时间
  * 每个设备占用一个 DMA 控制器，当设备增加时，需要增加新的 DMA 控制器

**与中断方式的区别**

* 中断控制方式在 *每个数据* 传送完成后中断 CPU，DMA 控制方式在传送 *一批* 数据完成之后中断 CPU
* 中断控制方式的数据传送在中断处理时由 CPU 控制完成，需要程序切换、保存和恢复现场。而 DMA 方式下由 DMA 控制器控制完成，只有开始和结束需要 CPU 干预，在传输过程中不需要 CPU 干预。
* 程序中断方式具有对 *异常事件* 的处理能力，而 DMA 控制方式适用于 *数据块* 的传输

#### 通道技术 (Channel)

> 原理与 DMA 类似。
>
> **通道** 是一个特殊功能的处理器，有自己的指令和程序专门负责数据输入输出的传输控制。CPU 将”传输控制“功能下放给通道，由通道负责”数据处理功能“。这样，通道与 CPU 分时使用内存，实现 CPU 内部运算与 I/O 设备的并行工作

* **基本思想**：进一步减少 CPU 干预

* I/O 通道专门负责输入输出，独立于 CPU，有自己的指令体系。可执行由通道指令组成的通道程序，因此可以进行较为复杂的 I/O 控制

* **优点**： 执行一个通道程序可以完成好几组 I/O 操作，与 DMA 相比减少了 CPU 干预

* **缺点**： 费用较高

* **通道种类**

  * **字节多路通道**：以字节为单位交叉工作

    > 当为一台设备传送一个字节后，立即转去为另一台设备传送一个字节；适用于打印机、终端等低速或中速的 I/O 设备

  * **数组选择通道**

    > 以 ”组方式“ 工作，每次传送一批数据，传送速率很高，但在一段时间只能为一台设备服务。

  * **数组多路通道**

    > 综合了字节多路通道和数据选择通道。对通道程序采用多道程序设计技术，使得与通道连接的设备可以并行工作

* **与 DMA 的区别**

  * 通道具有更强的处理 I/O 的功能
  * 一个通道可以同时控制多种设备

### I/O 软件的组成

#### I/O 软件设计

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607132407679-667178510.png)

#### 设备独立性

* 为了实现设备独立性引入 **逻辑设备** 和 **物理设备** 两个概念

* 系统需具有将逻辑设备名称转换为某物理设备名称的功能

* 优点

  * 设备分配更灵活
  * 易于实现 I/O 重定向

* 逻辑设备名到物理设备名的映射

  * 设备逻辑表 LUT(Logical Unit Table)

    > 包含逻辑设备名、物理设备名和设备驱动程序入口地址

  * 可以为整个系统，也可以为每个用户单独设置 LUT

#### 设备驱动程序

> 与设备密切相关的代码

* 接收来自设备无关的上层软件的抽象请求，并执行这个请求
* 向设备发送命令和参数，并监督它们正确执行

**组成**

* 自动配置和初始化子程序

  > 检查设备工作是否正常

* I/O 操作子程序

  > 系统调用的结果

* 中断服务子程序

  > 系统接收硬件中断，再由系统调用中断服务子程序

**设备驱动程序与应用程序的区别**

* 应用程序以 `main` 开始，而设备驱动程序以一个模块初始化函数作为入口
* 应用程序从头到尾执行一个任务，而设备驱动程序完成初始化后不再运行，等待系统调用
* 应用程序可以使用标准 C 函数库，而驱动程序则不能

### I/O 缓冲管理

#### 缓冲技术

缓冲技术可以提高外设利用率

* 原因
  * 匹配 CPU 与外设的不同处理速度
  * 减少对 CPU 的中断次数
  * 提高 CPU 和 I/O 设备之间的并行性

#### 单缓冲 (single buffer)

每当用户进程发出一个 I/O 请求时，操作系统便在主存中为之分配一个缓冲区

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607170753413-545748149.png)

#### 双缓冲 (double buffer)

两个缓冲区，CPU 和外设都可以连续处理而无需等待对方。

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607170729070-527687345.png)

要求 CPU 与外设的速度相近

#### 环形缓冲 (circular buffer)

当 CPU 与外设的处理速度相差极大时，引入多缓冲机制，将多个缓冲组织成循环缓冲形式

作为输入的多缓冲区可分为三种类型：

* **空缓冲区 R**：用于装输入数据
* **已装满数据的缓冲区 G**
* 计算进程 **正在使用的工作缓冲区 C**

作为输入的缓冲区可设置三个指针：

* 用于 **指示计算进程下一个可用缓冲区 G 的指针 Nextg**
* 用于 **指示输入进程下次可用的空缓冲区 R 的指针 Nexti**
* 用于 **指示计算进程增在使用的工作缓冲区 C 的指针 Current**

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607170642294-1380649775.png)

#### 缓冲池 (buffer pool)

将相同类型的缓冲区链成一个队列

* **空缓冲队列 emq**
* **输入队列 inq**
* **输出队列 outq**

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607170557640-1038523088.png)

**工作方式**

* **收容输入**

  在输入进程需要输入数据时，便调用 Getbuf(emq) 过程，从空缓冲队列的队首取出一个空的缓冲区，把它作为收容输入工作缓冲 hin。

  然后将数据输入其中

  装满后调用 Putbuf(inq, hin) 过程，将该缓冲区挂在输入队列上

* **提取输入**

  计算进程需要输入数据时，调用 Getbuf(inq) 过程，从输入队列队首去除一个缓冲区，作为提取输入工作缓冲区 sin

  计算进程从中提取数据

  使用完数据后，调用 Putbuf(emq, sin) 过程，将该缓冲区挂到空缓冲队列 emq 上

* **收容输出**

  计算过程需要输出时，调用 Getbuf(emq) 过程，从空缓冲区队列 emq 的队首取出一个空缓冲区，作为收容输出工作缓冲区 hout

  当其中装满输出后，又调用 Putbuf(outq, hout) 过程，将缓冲区挂在 outq 末尾

* **提取输出**

  由输出进程调用 Getbuf(outq) 过程，从输出队列队首取出一个装满输出数据的缓冲区，作为提取输出工作缓冲区 sout

  提取数据

  调用 Putbuf(emq, sout) 过程，将该缓冲区挂在空缓冲队列末尾

### I/O 设备管理

#### I/O 设备分配

#### 数据结构

1. **设备控制表(DCT, Device Control Table)**：每个设备一张，描述设备特性和状态。反映设备的特性、设备和控制器的连接情况。反应设备的特性、设备和控制器的连接情况

   ![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607194409648-1166416661.png)

   ![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607194050828-1327675172.png)

2. **控制器控制表 (COCT, COntroller Control Table)**

   每个设备控制器一张，描述 I/O 控制器的配置和状态。

   ![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607194331985-1238006993.png)

3. **通道控制表 (CHCT, CHannel Control Table)**

   每个通道一张，描述通道工作状态。

   ![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607194348426-1248032932.png)

4. **系统设备表 (SDT, System Device Table)**

   反映系统中设备资源的状态，记录所有设备的状态及其设备控制表的入口。SDT 表项的组成：

   * **DCT 指针**：指向相应设备的 DCT
   * **设备使用进程标识**：正在使用该设备的进程标识
   * **DCT 信息**：为引用方便而保存的 DCT 信息

#### 设备分配时应考虑的因素

* 设备固有属性：独享、共享、虚拟设备
* 设备分配算法：先来先服务、优先级高者优先
* 设备分配中的安全性：死锁问题
  * 安全分配（同步）
  * 不安全分配（异步）

#### 单（多）通路 I/O 系统的设备分配

一个设备对应一（多）个控制器，一个控制器对应一（多）个通道

* 分配设备
* 分配设备控制器
* 分配通道

#### ==用户空间的 I/O 软件：SPOOLing 技术==

> **SPOOLing, Simultaneous Peripheral Operation On Line**，也称虚拟设备技术，可把独享设备转变成具有共享特征的虚拟设备，从而提高设备利用率。

应用程序进行 I/O 操作时，只是和 SPOOLing 程序交换数据，可以称为 ”虚拟 I/O“。应用程序实际上是从 SPOOLing 程序的缓冲池中读出数据或把数据送入缓冲池，而不是跟实际的外设进行 I/O 操作。

**SPOOLing 系统组成**

* **输入井和输出井**：在磁盘上开辟的两大存储空间。输入井是模拟脱机输入时的磁盘设备，用于暂存 I/O 设备输入的数据。输出井是模拟脱机输出时的磁盘，用于暂存用户程序和输出数据
* **输入缓冲区和输出缓冲区**：缓和 CPU 与磁盘之间速度不匹配，在内存中开辟的两个缓冲区，输入缓冲区用于暂存由输入设备送来的数据，以后再传送到输入井，输出缓冲区用于暂存从输出井送来的数据，以后再传送给输出设备
* **输入进程 SPi 和输出进程 SPo**：利用两个进程模拟脱机 I/O 时的外围控制机。

### I/O 性能问题

#### I/O 操作的步骤

1. **磁盘把数据装载进内核的内存空间**
2. **内核的内存空间的数据 copy 到用户的内存空间中**

解决 I/O 性能问题的两个途径：

* 使 CPU 利用率尽可能不被 I/O 降低

  > 使用缓冲技术 *减少* 或 *缓解* 速度差异，同时使用异步 I/O 使 CPU 不等待 I/O

* 使 CPU 尽可能摆脱 I/O

  > 使用 DMA、通道等 I/O 部件让 CPU 摆脱 I/O 操作的影响

#### 阻塞 I/O

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607204657963-1682940241.png)

调用结果返回之前，当前进程会被挂起（线程进入睡眠状态），函数只有在得到结果之后，才会返回，才能继续运行

#### I/O 多路复用

工作进程调用一个管理 I/O 的特殊库函数，此库函数可以接收并处理多个 I/O 请求，工作进程可以同时等待多个 I/O 请求。阻塞在多个进程上，相较前者的效率更高。

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607204820676-1942297118.png)

#### 非阻塞 I/O 

进程发起 I/O 调用，I/O 通知进程进行别的操作

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607204933605-435903107.png)

#### 事件（信号）驱动 I/O

进程发起调用，通过回调函数，内核记住哪个进程申请的，在第一段完成后向进程发起通知，避免进程在第一段时忙等。第二段依然是阻塞的

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607204954435-1803283200.png)

#### 异步 I/O

无论第一段还是第二段，不再向系统调用提出任何反馈，只有数据完全复制到服务进程内存中以后，在向服务进程返回 Ok 的信息，其它时间，进程可以随意做自己的事情，直到内核通知 ok 信息。

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607215257429-1768857617.png)

#### 五种模型比较

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607215608609-58389590.png)

## 磁盘管理

### 磁盘的结构

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607222611978-1575846319.png)



> <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL21lbmd4aWFveHUvbWVuZ3hpYW94dS5naXRodWIuaW8vbWFzdGVyL3Bvc3QtaW1hZ2VzLzE1Njk2NzkyNjY4ODIuanBn?x-oss-process=image/format,png" style="zoom: 67%;" />
>
> 磁盘一般有一个或多个盘片。每个盘片可以有两面，即第一个盘片的正面为0面，反面为 1 面；第二个盘片的正面为 2 面…依次类推。磁头的编号也和盘面的编号是一样的，因此有多少个盘面就有多少个磁头。盘面正视图如下图，磁头的传动臂只能在盘片的内外磁道之间移动。因此不管开机还是关机，磁头总是在盘片上面。关机时，磁头停在盘片上面，抖动容易划伤盘面造成数据损失，为了避免这样的情况，所以磁头都是停留在起停区的，起停区是没有数据的。
>
> ![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL21lbmd4aWFveHUvbWVuZ3hpYW94dS5naXRodWIuaW8vbWFzdGVyL3Bvc3QtaW1hZ2VzLzE1Njk2NzkyOTQ5MjMuanBn?x-oss-process=image/format,png)
>
> 每个盘片的盘面被划分成多个狭窄的同心圆环，数据就存储在这样的同心圆环上面，我们将这样的圆环称为**磁道 (Track)**。每个盘面可以划分多个磁道，最外圈的磁道是0号磁道，向圆心增长依次为1磁道、2磁道…磁盘的数据存放就是从最外圈开始的。
>
> <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL21lbmd4aWFveHUvbWVuZ3hpYW94dS5naXRodWIuaW8vbWFzdGVyL3Bvc3QtaW1hZ2VzLzE1Njk2NzkzMjEzOTIuanBn?x-oss-process=image/format,png" alt="img" style="zoom:80%;" />
>
> 根据硬盘的规格不同，磁道数可以从几百到成千上万不等。每个磁道可以存储数 Kb 的数据，但是计算机不必要每次都读写这么多数据。因此，再把每个磁道划分为若干个弧段，每个弧段就是一个扇区 (Sector)。扇区是硬盘上存储的物理单位，现在每个扇区可存储 512 字节数据已经成了业界的约定。也就是说，即使计算机只需要某一个字节的数据，但是也得把这个 512 个字节的数据全部读入内存，再选择所需要的那个字节。
>
> ![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL21lbmd4aWFveHUvbWVuZ3hpYW94dS5naXRodWIuaW8vbWFzdGVyL3Bvc3QtaW1hZ2VzLzE1Njk2NzkzNTAzOTIuanBn?x-oss-process=image/format,png)
>
> **柱面**是我们抽象出来的一个逻辑概念，简单来说就是处于同一个垂直区域的磁道称为柱面 ，即各盘面上面相同位置磁道的集合。需要注意的是，磁盘读写数据是按柱面进行的，磁头读写数据时首先在同一柱面内从 0 磁头开始进行操作，依次向下在同一柱面的不同盘面(即磁头上)进行操作，只有在同一柱面所有的磁头全部读写完毕后磁头才转移到下一柱面。因为选取磁头只需通过电子切换即可，而选取柱面则必须通过机械切换。数据的读写是按柱面进行的，而不是按盘面进行，所以把数据存到同一个柱面是很有价值的。
>
> <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL21lbmd4aWFveHUvbWVuZ3hpYW94dS5naXRodWIuaW8vbWFzdGVyL3Bvc3QtaW1hZ2VzLzE1Njk2NzkzNzU4NzguanBn?x-oss-process=image/format,png" alt="img" style="zoom:80%;" />
>
> 磁盘被**磁盘控制器**所控制（可控制一个或多个），它是一个小处理器，可以完成一些特定的工作。比如将磁头定位到一个特定的半径位置；从磁头所在的柱面选择一个扇区；读取数据等。
>
> ————————————————
> 版权声明：本文为CSDN博主「Guanngxu」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/heuguangxu/article/details/80072024

#### 基本概念

##### 扇区 (sector)

盘面被分成许多扇形的区域

##### 磁道 (track)

盘片上以盘片中心为圆心，不同半径的同心圆

##### 柱面 (cylinder)

硬盘中，不同盘片相同半径的磁道所组成的援助

##### 磁头 (head)

每个磁盘由两个面，每个面都有一个磁头

#### 扇区的组织与内部结构

* 每个磁道的扇区数不是常量
* 绝大多数磁盘有一些缺陷扇区，因此映射必须用磁盘上其他空闲扇区来替代这些缺陷扇区

#### 磁盘缺陷

> **P 表**：又称为永久缺陷列表，用于记录硬盘生产过程中产生的缺陷
>
> **G 表**：又称增长缺陷列表，用于记录硬盘使用过程中由于磁介质性能变弱而引起的缺陷

* **固件区**：存储硬盘的固件（硬盘控制器使用）
* **工作区**：用户存储数据的区域，也就是硬盘标定容量的扇区
* **保留区**：超过固件定义的硬盘容量的那些扇区

### 磁盘的组织

#### 主引导扇区 (MBR)

>  硬盘的 0 柱面、0 磁头、1 扇区称为主引导扇区。

用于硬盘启动时将系统控制权交给用户指定的、在分区表中登记了某个操作系统分区。

包括：

* **启动代码** & **数据**

* **分区表**

* **幻数(Magic Number)**

  用于检查

#### 分区表 (DPT)

> 分区表由四个分区项组成，每个分区项数据为 16 字节，记录了启动时需要的分区参数

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210607224007151-1558900650.png)

#### 分区引导扇区 (DBR)

#### 容量

$$
磁盘地址与块号的转换\\
已知块号，则磁盘地址：\\
柱面号 = \frac{块号}{磁头数\times 扇区数}\\
磁头号 = \frac{块号 mod (磁头数\times 扇区数)}{扇区数}\\
扇区号 = (块号 mod （磁头数\times 扇区数)) mod 扇区数 \\
已知磁盘地址：\\
块号 = 柱面号 \times (磁头数 \times 扇区数)+磁头号\times 扇区数+扇区号
$$

#### ==访问时间==

##### 寻道时间

> 把磁臂（磁头）从当前位置移到指定磁道上所经历的时间。该事件是启动磁盘的时间 s 与磁头移动 n 条磁道所花费的时间之和

$Ts = m\times n + s$，其中 $m$ 为常数

##### 旋转延迟时间

> 磁盘旋转所花费的时间

$Tr = \frac{1}{2r}$，其中，磁盘旋转速度为 $r$

##### 传输时间

> 把数据从磁盘读出，或向磁盘写入数据所经历的时间

$Tt = \frac{b}{rN}$，其中 $b$ 为磁盘每次读/写的字节数，$r$ 为磁盘旋转速度，$N$ 为磁道上的字节数

##### 访问时间

> 磁盘访问数据的时间

访问时间 = 寻道时间 + 旋转延迟时间 + 传输时间

$Ta = Ts + \frac{1}{2r} + \frac{b}{rN}$

### ==磁盘的调度算法==

#### 先来先服务算法 (FCFS)

* **算法思想**

  按访问请求到达的先后次序服务

* **优点**

  简单、公平

* **缺点**

  效率低，服务时间长，对机械结构不利

#### 最短寻道时间优先算法 (SSTF, Shortest Seek Time First)

* **算法思想**

  优先旋转距当前磁头最近的访问请求进行服务，主要考虑寻道优先

* **优点**

  改善磁盘平均服务时间

* **缺点**

  可能产生“饥饿”现象

#### 扫描算法 (SCAN)

* **算法思想**

  当有访问请求时，磁头按一个方向移动，在移动过程中对遇到的访问请求进行服务，直到访问到最边缘，之后改变运动方向，继续移动，如此反复

* **优点**

  克服饥饿问题

* **缺点**

  两侧磁道被访问的频率低于中间磁道

#### 循环扫描算法 (CSCAN)

* **算法思想**

  * 按照所要访问的柱面位置的次序旋转访问者
  * 移动臂到达最后一个柱面时，立即带动读写磁头快速返回 0 号柱面
  * 返回时不为任何等待访问者服务
  * 返回后可再次扫描

* **优点**

  克服饥饿问题

  避免扫描算法中两侧磁道被访问的频率低的缺点

#### LOOK

SCAN + 只处理与当前移动方向相同的请求

#### C-LOOK

CSCAN + 只处理与当前移动方向相同的请求

#### N-Step-SCAN

将磁盘请求队列分成若干个长度为 N 的子队列，解决磁头粘贴问题

#### FSCANS

分为当前请求和新请求两个区域

### 磁盘空间的管理

#### 位图

用一串二进制位反映磁盘空间中分配使用情况，每个物理块对应 1 位，分配的物理块为 0，否则为 1.

#### 空闲表法

将所有空闲块记录在一个表中，称为空闲表。

主要记录两项内容：起始块号，块数

#### 空闲链表法

把所有空闲块链成一个表

#### 成组链接法

把空白物理块分成组，在通过指针把组与组之间链接起来

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608084048833-952725257.png)

优点：

1. 空白块号等级不占用额外空间
2. 节省时间
3. 采用后进先出的栈结构思想

### ==RAID==

> **RAID**，廉价冗余磁盘阵列(Redundant Arrays of Inexpensive Disks)

RAID 的基本思想是把多个相对便宜的硬盘组合起来，称为一个硬盘阵列组，使性能达到甚至超过一个价格昂贵，容量巨大的硬盘。

##### RAID0，条带化存储

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608090408026-1156753631.png)

##### RAID1，镜像存储

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608090427293-1271398581.png)

##### RAID2，海明码检验条带存储

> **海明码**是一种在原始数据中加入若干校验码来进行错误检测和纠正的编码技术，其中第 2n 位（ 1, 2, 4, 8, … ）是校验码，其他位置是数据码。

![image-20210608090507115](C:\Users\Ericaaaaaaaa\AppData\Roaming\Typora\typora-user-images\image-20210608090507115.png)

**特点**：

* 并行存取，各个驱动器同步工作
* 使用海明编码来进行错误检测和纠正，数据传输效率高
* 需要多个磁盘来存放海明校验码信息，冗余磁盘数量与数据盘数量的对数成正比
* 是一种在多磁盘易出错环境中的有效选择

##### RAID3，奇偶校验条带存储，共享校验盘，数据条带存储单位为字节

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608091741456-1642683777.png)

**特点**：

* 将磁盘分组，读写要访问组中所有盘，每组中有一个盘作为校验盘
* 校验盘一般采用奇偶检验
* 缺点：恢复时间长

##### RAID4，奇偶校验条带存储，共享校验盘，数据条带存储单位为块

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608092051313-679702965.png)

**特点**：

* 冗余代价与 RAID3 相同
* 访问数据的方法与 RAID3 不同
* 使用较少的磁盘参与操作，以使磁盘阵列可以并行进行多个数据的磁盘操作

##### RAID5，奇偶校验条带存储，校验数据分布式存储，数据条带存储单位为块

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608092249960-275974776.png)

**特点**：

* 更适用于小数据块和随机读写的数据
* 大部分传输只对一块磁盘操作，并可并行操作
* 有“写缺失”，即每次写操作将产生四个实际上的读/写操作，其中两次读旧的数据及奇偶信息，两次写新的数据及奇偶信息
* 当有**两块**盘损坏时，整个 RAID 数据失效

##### RAID6，奇偶检验条带存储

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608093206707-1284637403.png)

**特点**：

* 需要分配给奇偶校验信息更大的磁盘空间
* 写入数据需要访问 1 个数据盘和 2 个冗余盘，相对 RAID5 写损失更大
* **可以容忍双盘出错**
* 存储开销是 RAID5 的两倍

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608094941707-67533411.png)

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608095002914-1284449107.png)

### 提高 I/O 速度

提高 I/O 速度的主要途径：

* 选择性能更好的磁盘
* 并行化
* 采用适当的调度算法
* 设置磁盘高速缓冲区

#### 提高磁盘 I/O 速度：缓存

#### 优化数据布局

## 文件系统

### 基本概念

#### 文件

* 一组带标识（文件名）的，在逻辑上有完整意义的信息项序列
* **数据的存储和访问单位**
* **信息项**：构成文件内容的基本单位（单个字节，或多个字节），各信息项之间具有顺序关系
* 文件内容的意义：由文件建立者和使用者解释
* 文件包括：
  * **文件体**：文件本身的内容
  * **文件说明**：文件存储和管理的相关信息，如：文件名、文件内部标识、文件存储地址、访问权限、访问时间等
* 文件是一种 **抽象** 机制，提供了一种把信息保存在磁盘等存储设备上，并且便于以后访问的方法。
* 可以视为 **一个单独的连续的逻辑地址空间**，其大小即为文件的大小，与进程的地址空间无关

#### 文件管理

* 用户视角：使用 **逻辑文件**
* 操作系统视角：组织和管理 **物理文件**

#### 文件系统

##### 定义

> **文件系统** 是操作系统中统一管理信息资源的一种软件，管理文件的存储、检索、更新，提供安全可靠的共享和保护手段，并且方便用户使用

操作系统中与文件管理有关的那部分软件和被管理的文件以及实施管理所需要的数据结构的总体

##### 目的

为系统管理者和用户提供了对文件的透明存取（按名存取）

##### 任务目标

* 方便的文件访问
* 并发文件访问和控制
* 统一的用户接口
* 多种文件访问权限
* 执行效率
* 差错恢复

##### 文件系统要完成的任务

1. 统一管理磁盘空间，实现磁盘空间的分配与回收
2. 实现文件按名存取：名字空间 -- 映射 --> 磁盘空间
3. 实现文件信息的共享，并提供文件的保护、保密手段
4. 向用户提供一个方便使用、易于维护的接口，并向用户提供有关统计信息
5. 提高文件系统性能
6. 提供与 I/O 系统的统一接口

#### 文件名

> `文件名.扩展名`

#### 文件类型

* **按性质和用途**：系统文件、库文件、用户文件
* **按数据形式**：源文件、目标文件、可执行文件
* **按对文件实施的保护级别**：只读文件、读写文件、执行文件、不保护文件
* **按逻辑结构分**：有结构文件、无结构文件
* **按文件中物理结构分**：顺序文件、链接文件、索引文件

#### 文件的逻辑结构

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608101217180-1343124664.png)

##### 以字节为单位的流式结构

##### 记录式文件结构

##### 树形结构

#### 典型的文件结构

* **流式文件**：构成文件的基本单位是字符。文件是有逻辑意义、无结构的一串字符的集合
* **记录式文件**：文件由若干记录组成，可以按记录进行读写，查找等操作，每条记录由其内部结构

#### 文件存取方式

* 顺序存取
* 随机存取：提供读写位置（当前位置）

#### 文件存储介质

* 典型的存储介质：磁盘(包括 SSD)、磁带、光盘等
* 物理块：数据存储、传输和分配的单位，存储设备通常划分为大小相等的物理块，统一编号

#### 文件基本操作

创建、删除、打开、关闭、读、写、修改文件名、设置文件读写位置

#### 目录

> **目录** 是由文件说明索引组成的用于文件检索的特殊文件。目录的内容主要是文件访问和控制的信息（不包括文件内容）

##### 目录内容

* 基本信息
  * 文件名
  * 别名的数目
* 文件类型
  * 有/无结构
  * 内容
  * 用途
  * 属性
  * 文件组织
* 地址信息
  * 存放位置
  * 文件长度
* 访问控制信息
  * 文件所有者
  * 访问权限
* 使用信息
  * 创建时间
  * 最后一次读访问的时间和用户
  * 最后一次写访问的时间和用户

##### 目录操作

##### 目录分类

###### 单极文件目录

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608140317859-581897839.png)

* **文件目录的每个表目应包含：**
  * 文件符号名
  * 文件所在物理地址
  * 文件结构信息
  * 存取控制信息
  * 管理信息

* **特点**
  * 结构简单
  * 文件多时，目录检索时间长
  * 存在命名冲突问题
  * 不便于实现共享

###### 二级文件目录

在根目录下，每个用户对应一个目录（第二级目录），在用户目录下是该用户的文件，而不再有下级目录。适用于多用户系统，各用户可有自己的专用目录

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608140258846-598532483.png)

###### 多级文件目录（层次目录）

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608140720749-1593648631.png)

**实现方式**

* 在较高的目录级，其目录表包含下一级目录名以及一个指向其目录的指针
* 在最后一级目录，这个指针指向文件的物理地址

**访问方式**

* **绝对路径名**：从根目录开始依次经由各级目录名
* **相对路径**：结合当前路径的相对路径
* **当前目录**：也叫工作目录
* **上一级目录**：根目录的上级目录是它本身

**多级目录的特点**

* **层次清楚**
* **可解决文件重名问题**
* **查找速度快**
* **目录级别太多时，会增加路径检索时间**

### 实现方法

#### 文件控制块 (FCB, File Control Block)

为管理文件而设置的数据结构，保存管理文件所需的所有有关信息（文件属性或元数据）

* 基本信息
* 访问控制信息
* 使用信息

#### 文件常用属性

* 文件名
* 文件号
* 文件大小
* 文件地址
* 创建时间
* 最后修改时间
* 最后访问时间
* 保护口令
* 创建者
* 当前拥有者
* 文件类型
* 共享计数
* 各种标志（只读、隐藏、系统、归档、ASCII/二进制、顺序/随机访问、临时文件、锁）

#### ==文件逻辑结构和物理结构==

* 文件逻辑结构（文件组织）

* 文件物理结构

  文件在存储介质上的存放方式

  主要结构：连续、索引、串联

##### 连续结构

* 优点
  * 结构简单、实现容易，不需要额外的空间开销
  * 支持顺序和随机存取，顺序存取速度快
  * 连续存取时速度快
* 缺点
  * 文件长度一经固定便不易改变
  * 不利于文件的动态增加和修改
* 适用于变化不大的顺序访问的文件

##### 串联 / 链接文件结构

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210609184441018-31137670.png)

* 优点
  * 空间利用率高；能较好的利用辅助空间
  * 文件动态扩充和修改容易
  * 顺序存取效率高
* 缺点
  * 随机存取效率太低，如果访问文件最后的内容，实际上要访问整个文件
  * 可靠性问题：如指针出错
  * 链接指针可能占用一定的空间

##### 索引结构

系统为每个文件建立逻辑块号与物理块号的对照表，称为文件的索引表。文件由数据文件和索引表构成，这种文件称为索引文件

* 索引表位置：文件目录中，文件的开头等
* 索引表大小：固定大小，非固定大小

###### 索引文件结构

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608144534379-292679924.png)

###### 索引文件的操作

索引文件在存储区中占两个区：索引区和数据区

* 索引区存放索引表
* 数据区存放数据文件本身

访问索引文件需要两步操作：

* 查文件索引号，由逻辑块号查到物理块号
* 由此磁盘块号而获得要求的信息

###### 优缺点

* 优点
  * 既能顺序存取，又能随机存取
  * 满足文件动态增长、插入删除的要求
  * 能充分利用外存空间
* 缺点
  * 索引表本身带来系统开销

###### 索引表的组织

* **链接模式**

  一个盘一块索引表，多个索引表链接起来

* **多级索引**

  将一个大文件的所有索引表（二级索引）的地址放在另一个索引表（一级索引）中

* **综合模式**

  直接索引与间接索引方式的结合

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210609184943595-1813669943.png)

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210609185046041-753863180.png)

#### 文件存储器存储空间的管理

##### 空白文件目录

**目录的实现**

* 直接法：目录项 = 文件名 + FCB 信息

* 间接法：目录项 = 文件名 + FCB 地址

**长文件名的处理方法**

![image](https://img2020.cnblogs.com/blog/2324053/202106/2324053-20210608145344808-1739192984.png)

**符号文件目录的查询技术**

* 顺序查询法
* Hash 方法

**便于共享的目录组织**

硬连接 vs 软连接

> 【硬连接】
>
> 硬连接指通过索引节点来进行连接。在Linux的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号(Inode Index)。在Linux中，多个文件名指向同一索引节点是存在的。一般这种连接就是硬连接。硬连接的作用是允许一个文件拥有多个有效路径名，这样用户就可以建立硬连接到重要文件，以防止“误删”的功能。其原因如上所述，因为对应该目录的索引节点有一个以上的连接。只删除一个连接并不影响索引节点本身和其它的连接，只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放。也就是说，文件真正删除的条件是与之相关的所有硬连接文件均被删除。
>
> 【软连接】
>
> 另外一种连接称之为符号连接（Symbolic Link），也叫软连接。软链接文件有类似于Windows的快捷方式。它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的有另一文件的位置信息。

**保护文件的方法**

* 建立副本

  把同一个文件保存到多个存储介质上

  * 优点
    * 方法简单
  * 缺点
    * 设备费用和系统开销大

* 定时转储

  每隔一定时间把文件转储到其他存储介质上

* 规定文件权限

##### 空白物理块链

##### 位视图

#### 提高文件系统性能：块高速缓存

**块高速缓存(Block Cache)** 又称为文件缓存、磁盘高速缓存、缓冲区高速缓存。是指在内存中位磁盘块设置的一个缓冲区，保存了磁盘中某些块的副本

#### 基于日志结构的文件系统

##### LFS

* 将磁盘看作一个日志系统：log
* 日志 log 是一个数据结构，所有的写操作在日志头完成
* 文件顺序添加到磁盘上

##### UFS

### 文件系统实例分析

#### FAT 文件系统

* 簇（块）大小：1、2、3、8、16、32或64扇区。簇由若干个扇区组成，在一个文件卷中从 0 开始对簇编号
* 文件系统的数据记录在“引导扇区”中
* 文件分配表（FAT, File Allocation Table）的作用：描述簇的分配状态、标注下一簇的簇号等
* FAT 表项：2 字节(16 位)
* 目录项：32 字节
* 根目录大小固定

#### Linux 文件系统

##### **Ext2** 文件系统——索引节点

利用索引结点 (inode) 来描述文件系统的拓扑结构。在单个文件系统中，每个文件对应一个索引节点，而每个索引节点由一个唯一的整数标识符。文件系统中所有文件的索引节点保存在索引节点表中。

> 一个文件系统允许的inode节点数是有限的，如果文件数量太多，即使每个文件都是0字节的空文件，系统最终也会因为节点空间耗尽而不能再创建文件。

##### Ext2 文件系统——目录

Ext2 文件系统中的目录实际上是一种特殊文件，有对应的索引节点，索引节点指向的数据块中包含该目录中所有目录项（文件、目录、符号链接等），每个目录项有自己的索引节点


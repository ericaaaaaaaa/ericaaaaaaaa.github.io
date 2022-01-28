---
layout: post
title: Algorithm
subtitle: 算法设计与分析课程笔记
categories: Algorithms
tags: note algorithm
---

# Introduction

## Definition

### Problem Instance

A problem instance is any valid input to the problem.

### Algorithm

An **algorithm** is a well defined **computational procedure** that transforms inputs into outputs, achieving the desired input-output relationship.

### Correct Algorithm

A **correct algorithm** **halts** with the correct output for every input instance. We can then say that the algorithm **solves** the problem.

## Analyzing Algorithms

#### Predict Resource Utilization

* **Memory** (space complexity)

* **Running time** (time complexity)

  * depends on the speed of the computer
  * depends on the implementation details
  * depends on the input, especially on the size of the input

  Measure the running time as the number of **primitive operations** used by the algorithm. (mathematically elegant and machine-independent)

  Measure the running time as **a function of the input size**. Let `n` denote the input size and let `T(n)` denote the running time for input of size n.

### Three Kinds of Analysis

#### Worst Case

Commonly used

* Running time guarantee
* Fair comparison

#### Average Case

Used sometimes

* Need to assume distribution
* Analysis is complicated

#### Best Case

useless

### Comparing Time Complexity

#### $O$-notation (Upper bounds)

$f(n)=O(g(n))$: There exists constant $c>0$ and $n_0$ such that $f(n)\le c\cdot g(n)$ for $n\ge n_0$

#### $\Omega$-notation (Lower bounds)

$f(n)=\Omega(g(n))$: There exists constant $c>0$ and $n_0$ such that $f(n)\ge c\cdot g(n)$ for $n\ge n_0$

#### $\Theta$-notation (Tight bounds)

$f(n)=\Theta(g(n))$: $f(n)=O(g(n))$ and $f(n)=\Omega(g(n))$

#### Examples

* $\sum_{i=1}^{n}i = \frac{n(n+1)}{2}$
* $\sum_{i=1}^{n}i^2 = \frac{n(n+1)(2n+1)}{6}$
* $\sum_{i=1}^{n}\frac{1}{n}=O(\log n)$ (Harmonic Series，调和级数)
* $\log(n!)=\log(n) + \log(n-1) + ...+\log(1)=O(n\log n)$

### 递归分析：主定理法

$$
T(n) = 
\left\{
\begin{array}{l}
\Theta(f(n)) & if\ f(n)=\Omega(n^{\log_ba+\varepsilon})\\
\Theta(n^{\log_ba}\log n) & if\ f(n)=\Theta(n^{\log_b a}) \\
\Theta(n^{\log_ba}) & if\ f(n)=O(n^{\log_ba-\varepsilon})
\end{array}
\right.
$$

# Divide and Conquer Algorithms

## Step

**分而治之**

1. **分解**原问题
2. **解决**子问题
3. **合并**问题解

## Merge Sort 归并排序

归并排序：分解数组，递归求解，合并排序

### 算法流程

1. 将数组 A[1, n] 排序问题**分解**为 A[1, [n/2]] 和 A[[n/2]+1, n] 排序问题
2. **递归解决**子问题得到两个有序的子数组
3. 将两个有序子数组**合并**为一个有序数组

### 伪代码

![](/assets/images/post/merge_sort.png)

![](/assets/images/post/merge.png)

### 复杂度分析

$T(n) = O(n\log n)$

## Maximum Contiguous Subarray Problem 最大子数组

### 伪代码

![](/assets/images/post/MCS.png)

![](/assets/images/post/find_max.png)

### 复杂度

$f(n)=O(n\log n)$

## Counting Inversions 逆序计数

### 问题介绍

The total number of inversions in $\sum_{1\le i\le j\le n}$, namely
$$
X_{i, j} = 
\left\{
\begin{array}{rcl}
1 & A[i]>A[j]\\
0 & A[i]\le A[j]
\end{array}
\right.
$$

### 伪代码

![](/assets/images/post/merge_count.png)

![](/assets/images/post/sort_count.png)

### 复杂度

$T(n)=n\log n$

## Polynomial Multiplication 多项式乘法

### Description

![](/assets/images/post/def.png)

* **Input**: Assume that the coefficients a$_i$ and b$_i$ are stored in arrays A[0…n] and B[0…m]

### 伪代码

![](/assets/images/post/poly_mult1.png)

![](/assets/images/post/poly_mult2.png)

## QuickSort and Partition 快速排序与划分

### 算法思路

1. 选取固定位置主元 x （如尾元素）
2. 维护两个部分的右端点变量 i, j
3. 考察数组 A[j]，只和主元比较
   * 若 A[j] ≤ x，则交换 A[j] 和 A[i+1]，i, j 右移
   * 若 A[j] > x，则 j 右移
4. 把主元放在中间作分界线

![](/assets/images/post/t.png)

![](/assets/images/post/p.png)

### 伪代码

![](/assets/images/post/partition.png)

![](/assets/images/post/quick_sort.png)

### 复杂度

* 最好情况：$O(n\log n)$
* 最坏情况：$O(n^2)$

### 优化——随机选择主元

![](/assets/images/post/random_partition.png)

#### 期望时间复杂度

$O(n\log n)$

## Randomized Selection 随机化选择

### 次序选择问题

#### 形式化定义

![](/assets/images/post/k_min.png)

#### 算法思想

* 选取固定位置主元，小于主元的元素个数 q-p
  * 情况 1：k = q - p + 1，A[q] 为数组第 k 小元素
  * 情况 2：k < q - p + 1，在 A[p .. q - 1] 中寻找第 k 小元素
  * 情况 3：k > q - p + 1，在 A[q + 1 .. r] 中寻找第 k - (q - p + 1) 小元素

#### 伪代码

![](/assets/images/post/partition.png)

![](/assets/images/post/selection.png)

#### 复杂度

* 最好情况：$T(n) = O(n)$
* 最差情况：$T(n) = O(n^2)$

#### 优化算法——随机选择主元

![](/assets/images/post/rp.png)

![](/assets/images/post/rs.png)

期望时间复杂度：$O(n)$

## Supplement Topic of Sorting 排序问题补充主题

### Heapsort 堆排序

#### Heap

##### Definition

Heaps are "almost complete binary trees"

* All levels are full except possibly the lowest level
* If the lowest level is not full, then nodes must packed to the left
* The values of the node is at least the value of its parent. `A[Parent(i)] <= A[i]`

##### Operation

* **Insert** in $O(\log n)$ time
* **Extract-Min** in $O(\log n)$ time

##### Properties

* A heap of height h has between $2^h$ to $2^{h+1}-1$ nodes. Thus, an n-element heap has height $\Theta(\log n)$
* The structure is so regular, it can be represented in an array and no links are necessary

##### Array Implementation of Heap

* The root is in array position 1
* For any element in array position i
  * The left child is in position 2i
  * The right child is in position 2i+1
  * The parent is in position [i/2]

#### Heapsort

* Build a binary heap of n elements
  * the minimum element is at the top of the heap
  * insert n elements one by one
* Perform n Extract-Min operations
  * the elements are extracted in sorted order
  * each Extract-Min operation takes $O(\log n)$ time
* Total time complexity: $O(n\log n)$

### Lower Bound for Sorting 基于比较的排序下界

Any comparison-based sorting algorithm requires $\Omega(n\log n)$ comparisons.

#### Counting-Sort

![](/assets/images/post/counting_sort.png)

### Sorting in Linear Time 线性时间排序

# Dynamic Programming Algorithms

### 动态规划算法

* **问题结构分析**

  给出问题表示，明确原始问题

* **递推关系建立**

  分析最优结构，构造递推公式

* **自底向上计算**

  确定计算顺序，依次求解问题

* **最优方案追踪**

  记录决策过程，输出最优方案

### 0-1 Knapsack 0-1 背包问题

#### 输入

* n 个商品组成集合 0，每个商品有两个属性 $v_i$ 和 $p_i$，分别表示体积和价格
* 背包容量为 $C$

#### 输出

求解一个商品子集 $S\subseteq 0$，

* **优化目标**：令 $\max \sum_{i\in S}p_i$ 
* **约束条件**：$\sum_{i\in S}v_i\le C$

#### 递归函数

$$
KnapsackSR(h, i, c)=\\ \max\{KnapsackSR(h, i-1, c-v_i)+p_i, KnapsackSR(h, i-1, c)\}
$$

#### 算法复杂度

![](/assets/images/post/0-1knap.png)

#### 计算顺序

![](/assets/images/post/seq.png)

![](/assets/images/post/rec.png)

#### 伪代码

![](/assets/images/post/knap.png)

![](/assets/images/post/knapdp.png)

![](/assets/images/post/knapdp1.png)

#### 时间复杂度

$O(n\cdot C)$

### Maximum Contiguous Subarray II 最大连续数组 II

#### 问题定义

![](/assets/images/post/qq1.png)

#### 问题结构分析

$D[i]$：以 $X[i]$ 开头的最大子数组和

原始问题：$S_{max}=\max_{1\le i\le n}\{D[i]\}$

#### 递推关系建立

$$
D[i] = 
\left\{
\begin{array}{l}
X[i] + D[i+1] & if\ D[i+1] > 0\\
X[i] & if\ D[i+1] \le 0
\end{array}
\right.
$$

构造追踪数组 $Rec[1..n]$

#### 伪代码

![](/assets/images/post/mcsdp.png)

![](/assets/images/post/mcsdp1.png)

#### 时间复杂度

$T(n) = O(n)$

### Longest Common Subsequences 最长公共子序列

#### 问题描述

![](/assets/images/post/lcs.png)

#### 问题表示

$C[i, j]$：$X[1..i]$ 和 $Y[1..j]$ 的最长公共子序列长度

#### 递推关系建立

$$
C[i,j]=
\left\{
\begin{array}{l}
\max\{C[i-1,j], C[i,j-1]\} & x_i\not= y_j\\
C[i-1,j-1]+1 & x_i=y_j
\end{array}
\right.
$$

![](/assets/images/post/cls2.png)

![](/assets/images/post/rec1.png)

#### 伪代码

![](/assets/images/post/lcs1.png)

![](/assets/images/post/lcs2.png)

![](/assets/images/post/plcs.png)

#### 时间复杂度

$O(n\cdot m)$

### Longest Common Substrings 最长公共子串

#### 问题描述

![](/assets/images/post/lss.png)

#### 问题表示

$C[i,j]$：在 $X[1..i]$ 和 $Y[1..j]$ 中，以 $x_i$ 和 $y_j$ 结尾的最长公共子串 $Z[1..l]$ 的长度

#### 递推关系建立

$$
C[i,j]=
\left\{
\begin{array}{l}
0 & x_i\not=y_j\\
C[i-1,j-1]+1&x_i=y_j
\end{array}
\right.
$$

![](/assets/images/post/sll1.png)

#### 伪代码

![](/assets/images/post/sll2.png)

![](/assets/images/post/sll3.png)

![](/assets/images/post/sll4.png)

#### 时间复杂度

$O(n\cdot m)$

### Minimum Edit Distance 最小编辑距离

#### 问题描述

编辑操作

* 删除
* 插入
* 替换

![](/assets/images/post/med1.png)

#### 问题表示

$D[i,j]$：字符串 $s[1..i]$ 变成 $t[1..j]$ 的最小编辑距离
$$
D[i,j]=\min
\left\{
\begin{array}{l}
D[i-1,j]+1 & 删除\\
D[i,j-1]+1 & 插入\\
D[i-1,j-1]+
\left\{
\begin{array}{l}
0 & if\ s[i] = t[j]\\
1 & if\ s[i]\not= t[j]
\end{array}
\right.
\end{array}
\right.
$$

#### 递推关系建立

![](/assets/images/post/med2.png)

#### 伪代码

![](/assets/images/post/med3.png)

![](/assets/images/post/med4.png)

![](/assets/images/post/med5.png)

#### 时间复杂度

$O(mn)$

### Rod-Cutting 钢条切割

#### 问题描述

![](/assets/images/post/rc1.png)

#### 问题表示

$C[j]$：切割长度为 $j$ 的钢条可获得的最大收益

$rec[j]$：记录长度为 $j$ 钢条的最优切割方案

#### 递归结构建立

$$
C[j] = \max_{1\le i\le j-1}\{p[i] + C[j-i],p[j]\}
$$

#### 伪代码

![](/assets/images/post/rc2.png)

![](/assets/images/post/rc3.png)

#### 算法复杂度

$O(n^2)$

### Chain Matrix Multiplication 矩阵链乘法

#### 问题定义

![](/assets/images/post/mm1.png)

#### 问题描述

$D[i,j]$：计算矩阵链 $U_{i..j}$ 所需标量乘法的**最小次数**

#### 递归表达式建立

$$
D[i,j]=\min_{1\le k<j}(D[i,k]+D[k+1,j+p_{i-1}p_kp_j])
$$

![](/assets/images/post/mm2.png)

![](/assets/images/post/mm3.png)

![](/assets/images/post/m4.png)

#### 伪代码

![](/assets/images/post/mm4.png)

![](/assets/images/post/mm5.png)

![](/assets/images/post/mm6.png)

#### 时间复杂度

$O(n^3)$

# Greedy Algorithms

## 算法描述

* **提出贪心策略**

  观察问题特征，构造贪心选择

* **证明策略正确**

  假设最优方案，通过替换证明

## Fractional Knapsack 部分背包问题

### 问题定义

![](/assets/images/post/fkp1.png)

### 问题描述

### 伪代码

![](/assets/images/post/fkp2.png)

### 算法复杂度

$O(n\log n)$

## Huffman Coding Problem 赫夫曼编码

### 背景

* **编码树**
  * 顶点到左结点的边标记为 0，到右节点的边标记 1，通过编码方案构造编码树
  * 每条根到叶子的路径对应的每个字符的二进制串

### 问题定义

![](/assets/images/post/hc1.png)

### 伪代码

![](/assets/images/post/hc2.png)

### 算法复杂度

$O(n\log n)$

## Activity Selection Problem 活动选择问题

### 无权重

#### 问题定义

![](/assets/images/post/asp1.png)

#### 算法思路

选择最早结束的活动，可以给后面的活动留更大的空间。

#### 伪代码

![](/assets/images/post/asp2.png)

#### 算法复杂度

$O(n\log n)$

### 有权重

#### 问题定义

![](/assets/images/post/asp3.png)

#### 问题分析

$D[i]$：集合 $\{a_1,a_2,a_3,...,a_i\}$ 中不冲突活动的最大权限和

#### 递推关系建立

$$
D[i] = \max\{D[p[i]]+w_i,D[i-1] \}\\
Rec[i] = 
\left\{
\begin{array}{l}
1 & 选择活动 a_i\\
0 & 不选活动 a_i
\end{array}
\right.
$$

![](/assets/images/post/asp4.png)

#### 伪代码

![](/assets/images/post/asp5.png)

![](/assets/images/post/asp6.png)

![](/assets/images/post/asp7.png)

#### 算法复杂度

$O(n\log n)$

# Graph Algorithms

## Basic Concepts in Graphs

### 图的概念

#### 图的定义

**图**可以表示为一个二元组 $G=<V, E>$，其中

* $V$ 表示非空顶点集，其元素称为顶点 (Vertex)

* $E$ 表示边集，其元素称为边 (Edge)

  $e=(u,v)$ 表示一条边，其中 $u\in V,v\in V,e\in E$

#### 相邻和关联

##### 相邻 (Adjacent)

边 $(u,v)$ 连接的顶点 $u$ 和 $v$ **相邻**

##### 关联 (Incident)

边 $(u,v)$ 和其连接的顶点 $u$ (或 $v$) 相互关联

#### 度

##### 顶点的度 (Degree of a Vertex)

顶点 $v$ 的度 $deg(v)$ 是 $v$ 关联的边数

##### 图的度 (Degree of a Graph)

图 $G=<V,E>$ 的度，是图各顶点的度之和，$deg(G)=\sum_{v\in V}deg(v)$

#### 握手定理 (Handshaking Lemma)

* 无向图的度是边数的两倍

#### 路径 (Path)

* 图中一个顶点序列 $<v_0,v_1,...,v_k>$ 称为 $v_0$ 到 $v_k$ 的**路径**
* 路径包含顶点 $v_0,v_1,...,v_k$ 和边 $(v_0,v_1),(v_1,v_2),...,(v_{k-1}, v_k)$
* 存在路径 $<v_0,v_1,...,v_k>$，则 $v_0$ **可达** $v_k$
* 如果 $v_0,v_1,...,v_k$ 互不相同，则该路径的**简单**的

#### 环路 (Cycle)

* 如果路径 $<v_0,v_1,...,v_k>$ 中 $v_0=v_k$ 且至少包含一条边，则该路径构成**环路**。
* 如果 $v_0,v_1,...,v_k$ 互不相同，则该环路的**简单**的
* **无环图** (Acyclic Graph)：图中不存在环路

#### 连通 (Connectivity)

##### 连通

* 如果图的**任意对顶点互相可达**，则称该图是**连通**的，反之称为**非连通**

##### 连通分量 (Connected Components)

* 根据是否连通将顶点进行分组，相互可达的顶点集称为**连通分量**

#### 子图 (Subgraph)

##### 子图 (Subgraph)

* 如果 $V'\subseteq V,E'\subseteq E$，则称图 $G'=<V',E'>$ 是图 $G$ 的一个**子图**。

##### 生成子图 (Spanning Subgraph)

* 如果 $V'=V, E\subseteq E$，则称图 $G'=<V', E'>$ 是图 $G$ 的一个**生成子图**。

#### 树 Tree

##### 树 (Tree)

连通、无环图 $T=<V_T,E_T>$，树有 $|V_T|-1$ 条边

##### 森林 (Forest)

一至多棵树组成的无环图

### 图的表示

#### 邻接链表

* 图 $G=<V,E>$，其邻接链表由 $|V|$ 条链表的数组构成
* 每个顶点有一条链表，包含所有与其相邻的顶点
* 空间大小 $O(|V|+|E|)$

#### 邻接矩阵

* 图 $G=<V,E>$ 的邻接矩阵由 $|V|\times |V|$ 的二维数组 $A$ 构成，满足
  $$
  A_{ij}=
  \left\{
  \begin{array}{l}
  1 & (i,j)\in E\\
  0 & (i,j)\not\in E
  \end{array}
  \right.
  $$

* 空间大小 $O(|V|^2)$，$O(1)$ 判断是否有边

## Breadth-First Search (BFS，广度优先搜索)

### 辅助数组

![](/assets/images/post/bfs1.png)

### 伪代码

![](/assets/images/post/bfs2.png)

![](/assets/images/post/bfs3.png)

### 时间复杂度

$O(|V|+|E|)$

## Depth-First Search (DFS，深度优先搜索)

### 问题分析

![](/assets/images/post/dfs1.png)

### 伪代码

![](/assets/images/post/dfs2.png)

![](/assets/images/post/dfs3.png)

### 时间复杂度分析

$O(|V|+|E|)$

### 深度优先树

#### 深度优先树

顶点以前驱为祖先形成的树

#### 树边

深度优先搜索树中的边

#### 后向边

不是树边，但两顶点在深度优先树中是祖先后代关系

**对于无向图，非树边一定是后向边。**

**括号化定理**

* 点 $v$ 发现时刻和结束时刻构成区间 $[d[v], f[v]]$
* 任意两点 $v,w$ 必须满足以下情况之一：
  * $[d[v],f[v]]$ 包含 $[d[w], f[w]]$，$w$ 是 $v$ 的后代
  * $[d[w],f[w]]$ 包含 $[d[v],f[v]]$，$v$ 是 $w$ 的后代
  * $[d[v],f[v]]$ 和 $[d[w],f[w]]$ 完全不重合 $v$ 和 $w$ 均不是对方的后代

**白色路径定理**

* 在深度优先树中，顶点 $v$ 是 $w$ 的祖先 $\Leftrightarrow$ 在 $v$ 被发现前，从 $v$ 到 $w$ 存在全为白色顶点构成的路径

#### 前向边

不再深度优先树种，从祖先指向后代的边

#### 横向边

顶点不具有祖先后代关系的边

## Cycle Detection (环路检测)

### 问题定义

![](/assets/images/post/cd1.png)

### 伪代码

![](/assets/images/post/cd2.png)

![](/assets/images/post/cd3.png)

### 时间复杂度

$O(|V|+|E|)$

## Topological Sort (拓扑排序)

### 问题定义

![](/assets/images/post/tp1.png)

### 广度优先策略

#### 算法思想

* 完成入度为 0 点对应的事件
* 删除完成事件，产生新的入度为 0 的点，继续完成

#### 伪代码

![](/assets/images/post/tp2.png)

#### 事件复杂度

$O(|V|+|E|)$

### 深度优先策略

#### 算法思想

DFS 搜索的完成时刻逆序即为拓扑排序顺序

#### 算法伪代码

![](/assets/images/post/tp3.png)

![](/assets/images/post/tp4.png)

#### 算法时间复杂度

$O(|V|+|E|)$

## Strongly Connected Components (强连通分量)

### 问题定义

#### 强连通分量

* 一个强连通分量是顶点的子集
* 强连通分量种**任意两点相互可达**
* 满足**最大性**：加入新顶点，不保证相互可达

![](/assets/images/post/scc1.png)

#### $G^{SCC}$

把强连通分量看作一个点得到的有向图

**$G^{SCC}$一定是有向无环图**

#### $SCC_{Sink}$

$G^{SCC}$ 中出度为 0 的点

* $G^{SCC}$ 中存在至少一个 $SCC_{Sink}$
* 删除 $SCC_{Sink}$，会产生新的 $SCC_{Sink}$

### 算法步骤

1. 把边反向，得到反向图 $G^R$
2. 在 $G^R$ 上执行 DFS，得到顶点完成时刻顺序 $L$
3. 在 $G$ 上按 $L$ 逆序执行 DFS，得到强连通分量

### 伪代码

![](/assets/images/post/scc2.png)

![](/assets/images/post/scc3.png)

### 时间复杂度

$O(|V|+|E|)$

## Minimum Spanning Trees (最小生成树)

### 问题背景

#### 子图 Subgraph

如果 $V'\subseteq V$，$E'\subseteq E$，则称图 $G'=<V',E'>$ 是图 $G$ 的一个子图

#### 生成子图 Spanning Subgraph

如果 **$\bold{V'= V}$**，$E'\subseteq E$，则称图 $G'=<V',E'>$ 是图 $G$ 的一个生成子图

#### 生成树 Spanning Tree

连通且无环的生成子图

#### 最小生成树

权重最小的生成树

#### 安全边 Safe Edge

* $A$ 是某棵最小生成树 $T$ 边的子集，即 $A\subseteq T$
* $A\cup\{(u,v)\}$ 仍是 $T$ 边的一个子集，则称 $(u,v)$ 是 $A$ 的**安全边**
* 若每次向边集中新增安全边，可保证边集 $A$ 是最小生成树的子集

#### 割 Cut

图 $G=<V, E>$ 是一个连通无向图，割 $(S, V-S)$ 将图 $G$ 的顶点集 $V$ 划分为两个部分

#### 横跨 Cross

给定割 $(S,V-S)$ 和边 $(u,v)$，$u\in S,v\in V-S$，称边 $(u,v)$ 横跨割 $(S,V-S)$

#### 轻边 Light Edge

横跨割的所有边中，权重最小的边称为横跨这个割的轻边

#### 不妨害 Respect

如果一个边集 $A$ 中没有边横跨某割，则称该割不妨害边集 $A$

#### 安全边辨识定理

![](/assets/images/post/safe.png)

### 问题定义

![](/assets/images/post/st1.png)

![](/assets/images/post/com.png)

### Prim

#### 算法思想

![](/assets/images/post/prim.png)

#### 辅助数组

![](/assets/images/post/prim2.png)

#### 伪代码

![](/assets/images/post/prim3.png)

![](/assets/images/post/prim4.png)

#### 时间复杂度

$O(|V|^2)$

### 优化 Prim--采用优先队列

#### 伪代码

![](/assets/images/post/prim5.png)

![](/assets/images/post/prim6.png)

#### 时间复杂度

$O(|E|\cdot \log{|V|})$

### Kruskal

#### 算法思想

![](/assets/images/post/k1.png)

#### 伪代码

![](/assets/images/post/k3.png)

**高效判定和维护所选边的顶点是否在一棵子树**——**不相交集合**

![](/assets/images/post/s1.png)

![](/assets/images/post/s2.png)

![](/assets/images/post/s3.png)

#### 时间复杂度

$O(|E|\log{|V|})$

## Single Source Shortest Path (单源最短路径)

### 问题定义

![](/assets/images/post/ss1.png)

### Dijkstra

> 算法使用范围：边权为正的图

#### 辅助数组

![](/assets/images/post/ss2.png)

#### 算法思想

![](/assets/images/post/ss3.png)

![](/assets/images/post/ss4.png)

![](/assets/images/post/ss5.png)

#### 伪代码

![](/assets/images/post/d1.png)

![](/assets/images/post/d2.png)

#### 时间复杂度

$O(|V|^2)$

### 用优先队列优化 Dijkstra 算法

#### 伪代码

![](/assets/images/post/d3.png)

![](/assets/images/post/d4.png)

#### 时间复杂度

$O(|E|\cdot \log{|V|})$

### Bellman-Ford

#### 问题背景

当图中存在**负权边**时，Dijkstra 算法不适用

#### 问题定义

![](/assets/images/post/bf1.png)

#### 算法思想

![](/assets/images/post/bf2.png)

#### 伪代码

![](/assets/images/post/bf3.png)

![](/assets/images/post/bf4.png)

#### 时间复杂度

$O(|E|\cdot |V|)$

## All-Pairs Shortest Path (所有点对最短路径)

### 问题定义

![](/assets/images/post/ap1.png)

### 问题分析

$D[k,i,j]$：可以从前 $k$ 个点选点经过时，$i$ 到 $j$ 的最短距离

### 递推关系建立

$$
D[k,i,j]=\min
\left\{
\begin{array}{l}
D[k-1,i,j]\\
D[k-1,i,k]+D[k-1,k,j]
\end{array}
\right.
$$

### 伪代码

![](/assets/images/post/sp2.png)

![](/assets/images/post/ap3.png)

### 时间复杂度

$O(|V|^3)$

## Summary

![](/assets/images/post/short.png)

# Dealing with Hard Problems

## Definition

### Input Size

The **input size** of a problem is the **minimum number** of bits ({0, 1}) needed to encode the input of the problem.

### Decision Problem

A **decision problem** is a question that has two possible answers: **yes** and **no**.

> If $L$ is the problem and $x$ is the input, we often write $x\in L$ to denote a **yes** and $x\not\in L$ to denote a **no** answer.

### Optimization Problem

An **optimization problem** requires an answer that is an optimal configuration.

> An optimization problem usually has a corresponding decision problem.
>
> For almost all optimization problems there exists a corresponding simpler decision problem.
>
> Thus if we prove that a given problem is hard to solve efficiently, then it is obvious that the optimization problem must be (at least as) hard.

### Yes / No Input

An instance of a decision problem is called a **yes-input** (respectively no-input) if the answer to the instance is **yes** (respectively no).

### Polynomial-time

An algorithm is **polynomial-time** if its running time is $O(n^k)$, where $k$ is a constant independent of n, and n is the **input-size** of the problem that the algorithm solves.

> Whether you use $n$ or $n^\alpha$ (for fixed a > 0) as the input size, it will not affect the conclusion of whether an algorithm is polynomial time.
>
> Polynomial-time algorithm is "practical" and exponential-time algorithm is not.

### P

The class **P** consists of all **decision problems** that are solvable in **polynomial time**. That is, there exists an algorithm that will **decide** in polynomial time if any given input is a yes-input or a no-input.

### Certificate

A **certificate** is a specific object corresponding to a **yes-input**, such that it can be used to show that the input is **indeed** a yes-input.

### NP

The class **NP** consists of all **decision problems** such that, for each yes-input, there exists a **certificate** which allows one to verify in **polynomial time** that the input is indeed a yes input.

### Polynomial-time reducible

* Let $L_1$ and $L_2$ be two decision problems.

* A **polynomial-time reduction** from $L_1$ to $L_2$ is a transformation $f$ with the following two properties:

  1. $f$ transforms an input x for $L_1$ into an input $f(x)$ for $L_2$ such that: a yes-input of $L_1$ maps to a yes-input of $L_2$, and a no-input of $L_1$ maps to a no-input of $L_2$.
  2. $f(x)$ is computable in **polynomial time** (in size (x))

  If such $f$ exists, we say that $L_1$ is **polynomial-time reducible** to $L_2$, and write $L_1\le pL_2$

> If $L_2$ is polynomial-time algorithm, so is $L_1$

### NP-Complete (NPC)

The class **NPC** of **NP-Complete** problems consists of all decision problems $L$ such that

* $L\in NP$
* for every $L'\in NP$, $L'\le pL$

Intuitively, NPC consists of all the hardest problems in NP.

### NP-Hard

A problem $L$ is **NP-Hard** if problem in NPC can be **polynomially reduced** to it. (but $L$ does **not** need to be in NP)

## Theorem

1. If $L_1\le p L_2$ and $L_2\in P$, then $L_1\in P$
2. If $L_1\le p L_2$ and $L_2\le p L_3$, then $L_1\le p L_3$
3. If **there is** a polynomial-time algorithm for $L\in NPC$, then there is a polynomial-time algorithm for **every** $L'\in NP$
4. If **there is no** polynomial-time algorithm for $L\in NPC$, then there is  no polynomial-time algorithm for **every** $L'\in NPC$

## Example

### P

* 判断给定图是否为树
* DFS, BFS
* DMST，最小生成树决策类
* 2-SAT

### NP

* D-SubsetSum
* DVC (Decision Vertex Cover)
* SAT (Satisfiability)
* 3-SAT
* DMST
* DKnapsack

### NP-Complete

Knapsack

NPC

DCLIQUE

Decision Vertex Cover

Decision Independent Set

## Question

| Question           | Answer  |
| ------------------ | ------- |
| $P\subseteq NP $   | yes     |
| $NP\subseteq P$    | unknown |
| $NPC\subseteq NP $ | yes     |
| $P=NP $            | unknown |


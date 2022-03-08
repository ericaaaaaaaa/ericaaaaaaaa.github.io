---
layout: post
title: 机器学习
subtitle: 周志华《机器学习》读书笔记
categories: MachineLearning
tags: book-report artificial-intelligence machine-learning
---

# 概念

## 基本概念

### 机器学习

### 数据

#### 训练集

#### 验证集

#### 测试集

## 模型评估

### 错误率 (error rate)

分类**错误**的样本数占样本总数的比例称为“错误率”。

### 精度 (accuracy)

分类**正确**的样本数占样本总数的比例称为“精度”。

> 精度 = 1 - 错误率

### 误差 (error)

把学习器的实际预测输出与样本的真实输出之间的差异称为“误差”。

#### 训练误差 (training error) / 经验误差 (empirical error)

学习器再训练集上的误差。

#### 泛化误差 (generalization error)

学习器在新样本上的误差。

> 目标：得到泛化误差最小的学习器。

### 拟合

#### 欠拟合 (underfitting)

#### 过拟合 (overfitting)

### 模型参数

#### 超参数

算法的参数，一般数量较少。

#### 其它参数

模型的参数，一般数量较多。

## 模型

### 线性模型 (linear model) 

#### 概念

线性模型试图学得一个通过属性的线性组合来进行预测的函数，即

$$
f(x) = w_1x_1+w_2x_2+...+w_dx_d+b
$$

一般用向量形式写成

$$
f(x) = w^Tx+b
$$

其中 $w=(w_1;w_2;...;2_d$。 $w$ 和 $b$ 学得之后，模型就得以确定。

#### 优缺点

### 非线性模型

# 方法

## 模型评估

### 留出法 (hold-out)

将数据集 $D$ 划分为两个互斥的集合，其中一个作为训练集 $S$，另一个作为测试集 $T$，即 $D=S\cup T$, $S\cap T=\empty$，在$S$ 上训练出模型后，用 $T$ 来评估其测试误差，作为对泛化误差的估计。

> 需要注意的是，训练 / 测试集的划分要尽可能保持数据分布的一致性，避免因数据划分过程中引入额外的偏差而对最终结果产生映像。

由于存在多种划分方式对初始数据集 $D$ 进行分割，因此单次使用留出法的估计结果往往不够稳定可靠，在使用留出法时，一般采用若干次随机划分、重复进行实验评估后取平均值作为留出法的评估结果。

> 常见的做法是将大约 $2/3\sim 4/5$ 的样本用于训练、剩余样本用于测试。

### 交叉验证法 (cross validation)

先将数据集 $D$ 划分为 $k$ 个大小相似的互斥子集，即 $D=D_1\cup D_2\cup...\cup D_k$, $D_i\cap D_j=\empty (i\not= j)$，每个子集 $D_i$ 都尽可能保持数据分布的一致性，即从 $D$ 中通过分层采样得到。然后每次用 $k-1$ 个子集的并集作为训练集，余下的那个子集作为测试集；这样就可以获得 $k$ 组训练 / 测试集，从而可以进行 $k$ 次训练 / 测试，最终返回的是这 $k$ 个测试结果的均值。

交叉验证法评估结果的稳定性和保真性很大程度上取决于 $k$ 的取值，通常把交叉验证法称为**k 折交叉验证** (k-fold cross validation)。

> $k$ 最常用的取值是 10。

将数据集 $D$ 划分为 $k$ 个子集，随机使用不同的划分重复 $p$ 次，最终的评估结果是这 $p$ 次 $k$ 折交叉验证结果的均值，称为 $p$ 次 $k$ 折交叉验证。

#### 留一法 (Leave-One-Out, LOO)

假定数据集 $D$ 中包含 $m$ 个样本，若令 $k=m$，则得到交叉验证法的一个特例——留一法。

由于留一法不受随机样本划分方式的映像，因此在绝大多数情况下，留一法中被实际评估的模型与期望评估的用 $D$ 训练出来的模型很相似。

* **优点**
  * 留一法的评估结果往往被认为比较准确。
* **缺点**
  * 留一法在数据集较大的情况下的开销是巨大的。

### 自助法 (bootstrapping)

自助法直接以自助采样法 (bootstrapping sampling) 为基础。给定包含 $m$ 个样本的数据集 $D$，对它进行采样产生数据集 $D'$。每次随机从 $D$ 中挑选一个样本，将其拷贝放入 $D'$，然后再将该样本放回初始数据集 $D$ 中，使得该样本在下次采样时仍有可能被采到。上述过程重复执行 $m$ 次后，得到包含 $m$ 个样本的数据集 $D'$，这就是自助采样的结果。

$D$ 中有一部分样本会在 $D'$ 中多次出现，而另一部分样本不出现。估计样本在 $m$ 次采样中始终不被采到的概率是 $(1-\frac{1}{m})^m$，取极限得到
$$
\lim_{m\rightarrow \infty}(1-\frac{1}{m})^m = \frac{1}{e}\approx 0.368
$$
由上式可得，通过自助采样，初始数据集 $D$ 中约有 36.8% 的样本未出现在采样数据集 $D'$ 中。将 $D'$ 用作训练集，$D/D'$ 用作测试集；这样，实际评估的模型与期望评估的模型都使用 $m$ 个训练样本，而我们仍有数据总量约 $1/3$ 的，没在训练集中出现过的样本用于测试，这样的测试结果，亦称为“**包外估计**” (out-of-bag estimate)。

* **优点**
  * 自助法在数据集较小、难以有效划分训练 / 测试集时很有用。
  * 自助法能从初始数据集中产生多个不同的训练集，这对集成学习等方法有很大好处。
* **缺点**
  * 自助法产生的数据集改变了初始数据集的分布，会引入估计偏差。

### 调参与最终模型

## 性能度量 (performance measure)

衡量模型泛化能力的评价标准。

不同的任务需求会有不同的性能度量方式。

### 分类任务

#### 错误率与精度

##### 错误率

**错误率**是分类错误的样本数占样本总数的比例。

对于样例集 $D$，错误率 $E$ 定义为：
$$
E(f;D)=\frac{1}{m}\sum_{i=l}^{m}\mathbb{I}(f(x_i)\not= y_i)
$$

##### 精度

**精度**是分类正确的样本数占样本总数的比例。

对于样例集 $D$，精度 $acc$ 定义为：
$$
acc(f;D)=\frac{1}{m}\sum_{i=1}^{m}\mathbb{I}(f(x_i)=y_i)=1-E(f;D)
$$
更一般的，对于数据分布 $D$ 和概率密度函数 $p(\cdot)$，错误率和精度可以分别描述为
$$
E(f;D)=\int_{x\sim D}\mathbb{I}(f(x)\not= y)p(x)dx\\
acc(f;D)=\int_{x\sim D}\mathbb{I}(f(x)= y)p(x)dx = 1-E(f;D)
$$

#### 查准率、查全率与 F1

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/26/Precisionrecall.svg/700px-Precisionrecall.svg.png)

| 真实情况 \ 预测结果 | 正例            | 反例           |
| ------------------- | --------------- | -------------- |
| **正例**            | $TP$ （真正例） | $FN$（假反例） |
| **反例**            | $FP$（假正例）  | $TN$（真反例） |

##### 查准率 (precision)

预测正例中真实正例的占比。
$$
P = \frac{TP}{TP+FP}
$$

##### 查全率 (recall)

真实正例中预测正例的占比。
$$
P = \frac{TP}{TP+FN}
$$
查准率和查全率是一对矛盾的变量，查准率高时，查全率往往偏低。

##### P-R 曲线（查准率-查全率曲线）

根据学习器的预测结果对样例进行排序，从最有可能为正例的样本开始，到最不可能为正例的样本，按此顺序逐一将样本作为正例进行预测，每次可以计算出当前的查全率、查准率。以查准率为纵轴，以查全率为横轴作图，得到查准率-查全率曲线，简称“P-R 曲线”，显示该曲线的图称为“P-R 图”。

![](https://media.geeksforgeeks.org/wp-content/uploads/20190611002050/pr_roc.png)

P-R 曲线越靠外，模型效果越好。

###### 度量

* **平衡点**

  ”**平衡点**“ (Break-Even Point, BEP) 是查准率和查全率的一个性能度量，它是”**查准率 = 查全率**“时的取值。平衡点值越高，可以认为模型越优。

* **$F1$ 度量**

  F1 基于查准率与查全率的调和平均 (harmonic mean) 定义：
  $$
  \frac{1}{F1} = \frac{1}{2} \cdot (\frac{1}{P} + \frac{1}{R}) \\
  F1 = \frac{2\times P\times R}{P+R} = \frac{2\times TP}{样例总数+TP-TN}
  $$
  
* **$F_\beta$ 度量**

  在一些任务中，对查准率和查全率的重视程度有所不同，因此引入 $F1$ 度量的一般形式——$F_\beta$，它能够表达出对查准率 / 查全率的不同偏好，定义为查准率和查全率的加权调和平均：
  $$
  \frac{1}{F_\beta} = \frac{1}{1+\beta^2} \cdot (\frac{1}{P} + \frac{\beta^2
  }{R}) \\
  F_\beta = \frac{(1+\beta^2)\times P\times R}{(\beta^2\times P)+R}
  $$
  其中 $\beta>0$ 度量了查全率对查准率的相对重要性，$\beta=1$ 时退化为标准的 $F1$；$\beta>1$ 时查全率有更大影响；$0<\beta<1$ 时查准率有更大影响。

###### 全局

先在各混淆矩阵上分别计算出查准率和查全率，记为 $(P_1, R_1),(P_2,R_2),...,(P_n,R_n)$，再计算平均值，这样就得到“宏查准率” (macro-P)，“宏查全率” ("macro-R")，以及相应的“宏-$F1$” (macro-$F1$)
$$
macro{\textit-}P= \frac{1}{n}\sum_{i=1}^n P_i\\
macro{\textit-}R = \frac{1}{n}\sum_{i=1}^n R_i\\
macro{\textit-}F1 = \frac{2\times macro{\textit-}P\times macro{\textit-}R}{macro{\textit-}P+macro{\textit-}R}
$$

###### 局部

先将各混淆矩阵的对应元素进行平均，得到 $TP$、$FP$、$TN$、$FN$ 的平均值，分别记为 $\overline{TP}$、$\overline{FP}$、$\overline{TN}$、$\overline{FN}$，再基于这些平均值计算出“微查准率” ($micro{\textit-}P$)、“微查全率” ($micro{\textit-}R$) 和“微 $F1$” ($micro{\textit-}F1$)：
$$
micro{\textit-}P = \frac{\overline{TP}}{\overline{TP}+\overline{FP}}\\
micro{\textit-}R = \frac{\overline{TP}}{\overline{TP}+\overline{FN}}\\
micro{\textit-}F1 = \frac{2\timesmicro{\textit-}P\timesmicro{\textit-}R}{micro{\textit-}P+micro{\textit-}R}
$$

#### ROC 与 AUC

> 很多学习器是测试样本产生一个实值或概率预测，然后将这个预测值与一个分类域值 (threshold) 进行比较，若大于域值则分正类，否则为反类。这个分类域值直接决定了学习器的泛化能力。

ROC 的全称是“受试者工作特征”（Receiver Operating Characteristic）曲线。其**以“真正例率 (True Positive Rate, TPR)” 为纵轴**，**以“假正例率 (False Positive Rate, FPR)” 为横轴**。

$$
TPR = \frac{TP}{TP+TN}\\
FPR = \frac{FP}{TN+FP}
$$

显示 ROC 曲线的图称为“ROC”图。

* **图像分析**：ROC 图中对角线对应于“随机猜测”模型，而点 (0,1) 对应将所有正例排在所有反例之前的“理想模型”。
* **绘图过程**：给定 $m^+$ 个正例和 $m^-$ 个反例，根据学习器预测结果对样例进行排序，然后把分类域值设为最大，即把所有样例均预测为反例，此时真正例率和假正例率均为 0，在坐标 (0, 0) 处标记一个点。然后，将分类域值依次设置为每个样例的预测值，即依次将每个样例划分为正例，设前一个标记点坐标为 (x, y)，当前若为真正例，则对应标记点的坐标为 (x, y+$\frac{1}{m^+}$)，当前若为假正例，则对应标记点的坐标为 (x+$\frac{1}{m^-}$, y)，然后用线段连接相应点即得。
* **学习器比较**：比较 ROC 曲线下的面积，即 **AUC(Area Under ROC Curve)**，AUC 越大，则学习器效果越好。
  $$
  AUC = \frac{1}{2}+\sum_{i=1}^{m-1}(x_{i+1}-x_i)\cdot(y_i+y_{i+1})
  $$
  定义排序“损失” (loss) 为：
  $$
  l_{rank} = \frac{1}{m^+m^-}\sum_{x^+\in D^+}\sum_{x^-\in D^-}(\mathbb{I}(f(x^+)<f(x^-))+\frac{1}{2}\mathbb{I}(f(x^+)=f(x^-)))
  $$
  从几何意义上看，$l_{rank}$ 对应的是 ROC 曲线之上的面积，故有：
  $$
  AUC + l_{rank} = 1
  $$

#### 代价敏感错误率与代价曲线

> 在现实任务中，不同类型的错误所造成的后果不同。，为权衡不同类型错误所造成的不同损失，可为错误赋予“非均等代价” (unequal cost)。

根据任务的领域知识设定一个“代价矩阵” (cost matrix)，其中 $cost_{ij}$ 表示将第 $i$ 类样本预测为第 $j$ 类样本的代价。一般来说，$cost_{ii}=0$

> 一般情况下，重要的是 $cost$ 之间的*比值*而非*绝对值*。

目标：最小化“总体代价” (total cost)。

以二分类为例，将第 0 类作为正类，第 1 类作为反类，$D^+$ 与 $D^-$ 分别代表样例集 $D$ 的正例子集和反例子集，则“代价敏感” (cost-sensitive) 错误率为：

$$
E(f;D;cost) = \frac{1}{m}(\sum_{x_i\in D^+}\mathbb{I}(f(x_i)\not ={y_i})\times cost_{01} +
                          \sum_{x_i\in D^-}\mathbb{I}(f(x_i)\not ={y_i})\times cost_{10})
$$

在非均等代价下，ROC 曲线不能直接反映出学习器的期望总体代价，而“代价曲线” (cost curve) 则可以达到该目的，代价曲线图的横轴是取值为 [0, 1] 的正例概率代价

$$
p(+)cost = \frac{\times cost_{01}}{p\times cost_{01}+(1-p)\times cost_{10}}
$$

其中 $p$ 的样例为正例的概率；纵轴是取值为 [0, 1] 的归一化代价。

$$
cost_{norm} = \frac{FNR\times p\times cost_{01}+FPR\times(1-p)\times cost_{10}}{p\times cost_{01}+(1-p)\times cost_{10}}
$$

### 聚类任务

## 比较检验

**统计假设检验** (hypothesis test) 为我们的学习器性能提供了重要依据。

> 在下述部分以错误率为性能度量，用 $\epsilon$ 表示。

### 假设检验

> 假设检验中的“假设”是对学习器泛化错误率分布的某种判断或猜想。

可根据测试错误率估推出泛化错误率的分布。

泛化错误率为 $\epsilon$ 的学习器在一个样本上犯错的概率是 $\epsilon$；测试错误率 $\hat{\epsilon}$ 意味着在 $m$ 个测试样本中恰有  $\hat{\epsilon}\times m$ 个被误分类。假定测试样本是从样本总体分布中独立采样而得，那么泛化错误率为 $\epsilon$ 的学习器将其中 $m'$ 的样本误分类、其余样本全部分类正确的概率为 $\left(\begin{array}{l}m\\m'\end{array}\right)\epsilon^{m'}(1-\epsilon)^{m- m'}$ ；由此可估算出其恰好将 $\hat{\epsilon}\times m$ 个样本误分类的概率如下式所示，这也表达了在 $m$ 个样本集的测试集上，泛化错误率为 $\epsilon$ 的学习器被测得测试错误率为 $\hat{\epsilon}$ 的概率：

$$
P(\hat{\epsilon};\epsilon) = 
\left(
  \begin{array}{l}
  m\\
  \hat{\epsilon}\times m  \end{array}
\right)
\epsilon^{\hat{\epsilon}\times m}(1-\epsilon)^{m- \hat{\epsilon}\times m}
$$

给定测试错误率，则解 $\partial P(\hat{\epsilon};\epsilon)/\partial\epsilon$ 可知，$P(\hat{\epsilon};\epsilon)$ 在 $\epsilon=\hat{\epsilon}$ 时最大，$|\epsilon-\hat{\epsilon}|$ 增大时 $P(\hat{\epsilon}, \epsilon)$ 减小。这符合**二项分布 (binomial)** 分布。

考虑假设 “$\epsilon\le\hat{\epsilon}$”，则在 $1-\alpha$ 的概率内所能观测到的最大错误率如下：

$$
\overline{\epsilon} = \min\epsilon\qquad s.t.\qquad \sum_{i=\epsilon_0\times m+1}^{m}
\left(
\begin{array}{c}
  m\\
  i
\end{array}
\right)\epsilon^i(1-\epsilon)^{m-i}\alpha
$$

其中 $1-\alpha$ 反映了结论的“置信度” (confidence)。

此时若测试错误率 $\hat{\epsilon}$ 小于临界值 $\overline{\epsilon}$，则根据二项检验可得出结论：在 $\alpha$ 的显著度下，假设 “$\epsilon\le\epsilon_0$” 不能被拒绝，即能以 $1-\alpha$ 的置信度认为，学习器的泛化错误率不大于 $\epsilon_0$；否则该假设可被拒绝，即在 $\alpha$ 的显著度下可认为学习器的泛化错误率大于 $\epsilon_0$。

在很多时候我们并非仅做一次留出法估计，而是通过多次重复留出法或是交叉验证法等进行多次训练 / 测试，这样会得到多个测试错误率，此时可使用 “t-检验” (t-testr)。假定我们得到了 $k$ 个测试错误率，$\hat{\epsilon}_1$，$\hat{\epsilon}_2$，...，$\hat{\epsilon}_k$，则平均测试错误率 $\mu$ 和方差 $\sigma^2$ 为

$$
\mu = \frac{1}{k}\sum_{i=1}^{k}\hat{\epsilon}_i\\
\sigma^2 = \frac{1}{k-1}\sum_{i=1}^{k}(\hat{\epsilon_i}-\mu)^2
$$

可以考虑到这 $k$ 个测试错误率可看作泛化错误率 $\epsilon_0$ 的独立采样，则变量

$$
\tau_t = \frac{\sqrt{k}(\mu-\epsilon_0)}{\sigma}
$$

服从自由度为 $k-1$ 的 $t$ 分布。

### 交叉验证 t 检验

对两个学习器 $A$ 和 $B$,若我们使用 $k$ 折交叉验证法得到的错误率分别为 $\epsilon_1^A,\epsilon_2^A,...,\epsilon_k^A$ 和 $\epsilon_1^B,\epsilon_2^B,...,\epsilon_k^B$。可用 $k$ 折交叉验证“成对 t 检验” (paired-tests) 来进行比较检验。这里的基本思想是若两个学习器的性能相同，则它们使用相同训练 / 测试集得到的测试错误率应相同，即 $\epsilon_i^A=\epsilon_i^B$。

具体的来说，对 $k$ 折交叉验证所产生的 $k$ 对测试错误率：先对每对结果求差，$\delta_i=\epsilon_i^A-\epsilon_i^B$；若两个学习器性能相同，则差值均值应为 0.因此，可根据差值 $\delta_1,\delta_2,...,\delta_k$ 来对“学习器 $A$ 与 $B$ 性能相同”这个假设做 $t$ 检验，计算出差值的均值 $\mu$ 和方差 $\sigma^2$，在显著度 $\alpha$ 下，若变量

### McNemar 检验

### Friedman 检验与 Nemenyi 后续检验

## 偏差与方差

# 线性模型


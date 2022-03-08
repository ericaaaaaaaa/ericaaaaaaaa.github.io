---
layout: post
title: 神经网络的可解释性
subtitle: 参考博客：https://jishuin.proginn.com/p/763bfbd3b58c
categories: MachineLearning
tags: book-report artificial-intelligence machine-learning
---

<center><h1>神经网络的可解释性</h1></center>

> 参考博客：https://jishuin.proginn.com/p/763bfbd3b58c

## CNN 可解释化

### 特征图可视化方法

特征图可视化有两类方法，一类是直接将某一层的 feature map 映射到 0-255 的范围，变成图像。另一类是使用一个反卷积网络（反卷积、反池化）将 feature map 变成图像，从而达到可视化 feature map 的目的。

#### 直接可视化

使用 `torchvision.utils.make_grid()` 函数实现归一化，将 feature map 标准化到 0-255 范围内。

```python
def make_grid(tensor, nrow=8, padding=2, normalize=True, range=None, scale_each=False,pad_value=0):
```

[参考链接](https://zhuanlan.zhihu.com/p/60753993)

#### 反卷积网络 (deconvnet)

图像像素经过神经网络映射到特征空间，而反卷积网络可以将 feature map 映射回像素空间。

##### 反池化 (unpooling)

将最大值放在原位置，其它位置直接置为 0。

##### 修正 (Rectification)

使用 ReLU 使得 unpooling 后的值都是正的。

##### 反卷积 (Filtering)

使用原网络的卷积核的转置作为卷积核，对 Rectification 后的输出进行卷积。

##### 导向反向传播 (Guided-backpropagation)

导向反向传播与反卷积网络的区别在于对 ReLU 的处理方式。在反卷积网络中使用 ReLU 处理梯度，只回传梯度大于 0 的位置，而在普通反向传播中只回传 feature map 中大于 0 的位置，在导向反向传播中结合这两者，只回传输入和梯度都大于 0 的位置，这相当于在普通反向传播的基础上增加了来自更高层的额外的指导信号，这阻止了负梯度的反传流动，梯度小于 0 的神经元降低了正对应更高层单元中我们想要可视化的区域的激活值。

### 卷积核可视化

#### 卷积核可视化原理

随机初始化生成一张图（指的是对像素值随机取值，不是数据集中随机选一张图），然后经过前向传播到该层，我们希望这个随机生成的图在经过这一层卷积核时，它的响应值能尽可能的大，换句话说，响应值比较大的图像是这个卷积核比较认可的，是与识别任务更相关的。然后不断调整图像像素值，直到响应值足够大，我们就可以认为此时的图像就是这个卷积核所认可的，从而达到可视化该卷积核的目的。

### 类可视化

这个主要用于确定图像哪些区域对识别某个类起主要作用。如常见的热力图（Heat Map），在识别猫时，热力图可直观看出图像中每个区域对识别猫的作用大小。这个目前主要用的方法有CAM系列（CAM、Grad-CAM、Grad-CAM++）。

#### CAM

CAM 的结构由 CNN 特征提取网络，全局平均池化 GAP，全连接层和 Softmax 组成。

实现原理：一张图片在经过 CNN 特征提取网络后得到 feature maps, 再对每一个 feature map 进行全局平均池化，变成一维向量，再经过全连接层与 softmax 得到类的概率。

假定在 GAP 前是 n 个通道，则经过 GAP 后得到的是一个长度为 1x n 的向量，假定类别数为 m，则全连接层的权值为一个 n x m 的张量。（注：这里先忽视 batch-size）

对于某一个类别 C, 现在想要可视化这个模型对于识别类别 C，原图像的哪些区域起主要作用，换句话说模型是根据哪些信息得到该图像就是类别 C。

做法是取出全连接层中得到类别 C 的概率的那一维权值，用 W 表示，即上图的下半部分。然后对 GAP 前的 feature map 进行加权求和，由于此时 feature map 不是原图像大小，在加权求和后还需要进行上采样，即可得到 Class Activation Map。
$$
M_c(x, y) = \sum_kw_k^cf_k(x, y)
$$

> CAM 有个很致命的缺陷，它的结构是由 CNN + GAP + FC + Softmax 组成，也就是说如果想要可视化某个现有的模型，但大部分现有的模型没有 GAP 这个操作，此时想要可视化便需要修改原模型结构，并重新训练，相当麻烦，且如果模型很大，在修改后重新训练不一定能达到原效果，可视化也就没有意义了。
>
> 因此，针对这个缺陷，其后续有了改进版 Grad-CAM。

#### Grad-CAM

原理：同样是处理 CNN 特征提取网络的最后一层 feature maps。Grad-CAM 对于想要可视化的类别 C，使最后输出的类别 C 的概率值通过反向传播到最后一层 feature maps，得到类别 C 对该 feature maps 的每个像素的梯度值，对每个像素的梯度值取全局平均池化，即可得到对 feature maps 的加权系数 alpha，论文中提到这样获取的加权系数跟 CAM 中的系数几乎是等价的。接下来对特征图加权求和，使用 ReLU 进行修正，再进行上采样。

使用 ReLU 的原因是对于那些负值，可认为与识别类别 C 无关，这些负值可能是与其他类别有关，而正值才是对识别 C 有正面影响的。

用公式表示如下：
$$
\alpha_k^c=\frac{1}{Z}\sum_i\sum_j\frac{\part y^c}{\part A_{ij}^k}\\
L_{Grad-CAM}^c=ReLU(\sum_k\alpha_k^cA^k)
$$

> Grad-CAM后续还有改进版Grad-CAM++，其主要的改进效果是定位更准确，更适合同类多目标的情况，所谓同类多目标是指一张图像中对于某个类出现多个目标，例如七八个人。

#### 参考链接

CAM: arxiv.org/pdf/1512.0415

Grad-CAM: arxiv.org/pdf/1610.0239

Grad-CAM++: arxiv.org/pdf/1710.1106

### 可视化工程与项目

通过一些研究人员开源出来的工具可视化CNN模型某一层。

#### CNN-Explainer

> 这是一个中国博士发布的名叫CNN解释器的在线交互可视化工具。主要对于那些初学深度学习的小白们 理解关于神经网络是如何工作很有帮助，如卷积过程，ReLU过程，平均池化过程，中间每一层的特征图的样子，都可以看到，相当于给了一个显微镜，可以随意对任意一层，任何一项操作的前后变化，观察得清清楚楚。

https://github.com/poloclub/cnn-explainer

#### 一些可视化特征图、卷积核、热力图的代码

可视化特征图

https://github.com/waallf/Viusal-feature-map

可视化卷积核

https://keras.io/examples/vision/visualizing_what_convnets_learn/

https://blog.keras.io/how-convolutional-neural-networks-see-the-world.html

Grad-CAM

https://github.com/ramprs/grad-cam

热力图

https://github.com/heuritech/convnets-keras

下面这个项目是同时包含特征图可视化，卷积核可视化和热力图的一个链接：

https://github.com/raghakot/keras-vis

#### 结构可视化工具

##### Netscope

用于可视化模型结构的在线工具，仅支持caffe的prototxt文件可视化。需要自己写prototxt格式的文件。

项目地址：https://github.com/ethereon/netscope

##### ConvNetDraw

项目地址：https://github.com/cbovar/ConvNetDraw

##### PlotNeuralNet

项目地址：https://github.com/HarisIqbal88/PlotNeuralNet

### 网络结构手动画图工具

PPT, VISIO，NN-SVG（在线工具）

项目地址：http://alexlenail.me/NN-SVG/

## 参考论文

1. 《Visualizing and Understanding Convolutional Networks》
2. 《Striving for Simplicity：The All Convolutional Net》
3. Learning Deep Features for Discriminative Localization
4. Grad-CAM: Why did you say that?Visual Explanations from Deep Networks via Gradient-based Localization
5. Grad-cam++: Generalized gradient-based visual explanations for deep convolutional networks


# Note: Deep Belief Network
深度置信网(Deep Belief Network)是人工神经网络的一种变种, 它是一种含有随机, 隐藏变量的概率生成模型. [深度置信网的逐层快速训练(A fast learning algorithm for deep belief nets)](https://www.cs.toronto.edu/~hinton/absps/fastnc.pdf)由*G.Hinton*在2006年提出. 在人工神经网络的架构上加上多个限制玻尔兹曼机的预训练就是一个深度置信网. 对人工神经网络不熟悉的可以参考[这里](ann.md). 

<br>
## 概要
* [结构](#结构)
* [限制玻尔兹曼机](#限制玻尔兹曼机)
* [预训练](#预训练)

<br>
## 结构
和人工神经网络相同, 深度置信网由底部可见层, 多个隐藏层和一个顶层分类器组成. 可见层观察到的是输入特征, 隐藏层负责解析隐藏特征, 顶层分类器进行分类. 除了可见层, 其他神经元都是随机二元的来增加神经网络的稀疏性. 在这个模型中我们不难看出, 除了顶部的分类器, 深度置信网的其他层其实是一个非监督学习的过程, 它可以用来解析隐藏特征, 聚类或者降维.

![dbn](https://s3-us-west-1.amazonaws.com/owenbdbn/DBN.gif")

深度置信网的训练分为预训练(Pre-train)和反向传播两部分, 深度神经网络的反向传播被称作优化(Fine-tune). 与人工神经网络最大的不同是, 深度神经网络的预训练叠加了多个玻尔兹曼机(Restricted Boltzmann Machine). 我们都知道, 对于一个神经网络而言, 权重的初始值相当重要, 而预训练的目的就是为整个神经网络提供一组较为优秀的初始权重. 预训练完之后的反向传播同普通人工神经网络类似.

## 限制玻尔兹曼机
<img src="https://s3-us-west-1.amazonaws.com/owenbdbn/rbm.jpg" height="500>

<br>
## 

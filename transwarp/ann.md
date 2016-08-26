# Note: Artificial Neural Network
**人工神经网络**是受到了生物学上动物的中枢神经系统的启发而开发出来的一种计算模型.
**人工神经网络**最早于20世纪40年代提出, 但由于庞大的神经网络和当时计算能力的局限, 神经网络的研究一直停滞不前.
直到20世纪80年代**分散式并行处理**的流行和由*Paul Werbos*创造的反向传播算法, 神经网络渐渐又开始流行起来. 并于21世纪开始同**深度学习**一起成为机器学习领域的热点.

<br>
## 概要
* [结构](#结构)
* [前向传播](#前向传播)
* [反向传播](#反向传播)
* [主流模型](#主流模型)

<br>
## 结构
人工神经网络由一个输入可见层, 多个隐藏层和一个分类输出层组成. 每一层由不同数目的神经单元组成, 前后两层之间的weight和bias组成了整个模型.

<img src="http://cs231n.github.io/assets/nn1/neural_net2.jpeg" height="240">

<br>
## 前向传播
对人工神经网络进行训练时, 我们首先把输入放入输入可见层, *喂*进神经网络, 并逐层传递.

<img src="http://ufldl.stanford.edu/tutorial/images/Network331.png" height="360">

### 组合函数
每一层的神经单元由上一层的单元和权重生成.

<img src="http://ufldl.stanford.edu/tutorial/images/SingleNeuron.png" height="180">

传递函数: $p(x_j^2) = \sigma(\Sigma w_{ij}^{12}x_i^1)$

### 激活函数
- Sigmoid: $\sigma(z) = {1 \over 1 + e^{-z}}$
- Tanh: $\sigma(z) = {sinh(z) \over cosh(z)} = {{e^z - e^{-z}} \over {e^z + e^{-z}}}$
- ReLU: $\sigma(z) = max(0, z)$

<br>
## 反向传播
因为多层的结构, 当进行迭代更新的时候, 输出层产生的error会反向传遍整个网络.

<img src="http://i.stack.imgur.com/H1KsG.png" height="360">

- 输出层的error就是分类器的error: $\delta_i^n = \sigma_i^n - y_i$
- 前一层的error由后一层的error产生: $\delta_i^n = \Sigma_j w_{ij}^{n+1} \delta_j^{n+1}$
- 更新权重使用梯度下降: $\Delta w_{ij} = -\gamma \sigma_i^n \delta_j^{n+1}$

有兴趣的可以看一下**[R. Rojas: Neural Networks, Springer-Verlag, Berlin, 1996](https://page.mi.fu-berlin.de/rojas/neural/chapter/K7.pdf)**中关于反向传播的部分.

<br>
## 主流模型
- 编码网络(AutoEncoder)
- 深度玻尔兹曼机器(DBM)
- [卷积神经网络(CNN)](cnn.md)
- [深度置信网(DBN)](dbn.md)
- [递归神经网络(RNN)](rnn.md)



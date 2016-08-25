# Note: Artificial Neural Network
**人工神经网络**是受到了生物学上动物的中枢神经系统的启发而开发出来的一种计算模型.
**人工神经网络**最早于20世纪40年代提出, 但由于庞大的神经网络和当时计算能力的局限, 神经网络的研究一直停滞不前.
直到20世纪80年代**分散式并行处理**的流行和由*Paul Werbos*创造的反向传播算法, 神经网络渐渐又开始流行起来. 并于21世纪开始同**深度学习**一起成为机器学习领域的热点.

### 概要
* [结构](#结构)
* [前向传播](#前向传播)
* [反向传播](#反向传播)
* [主流模型](#主流模型)

### 结构
![结构](http://cs231n.github.io/assets/nn1/neural_net2.jpeg)

### 前向传播
#### 组合函数
p = σ(wx + b)
#### 激活函数
![激活函数](http://ufldl.stanford.edu/tutorial/images/SingleNeuron.png)
- Sigmoid: ![sigmoid](http://file.scirp.org/Html/htmlimages%5C10-9402081x%5C34ca7298-136f-4b25-a192-1e7f24401b53.png)
- Tanh: ![tanh](http://folk.ntnu.no/oyvinbra/gdsp/images/tanh_formula.png)
- ReLU: ![relu](http://wiki.ubc.ca/images/math/a/2/c/a2c0a83b286b4d0ca1a01d23cf2d8f79.png)

### 反向传播

### 主流模型
- 编码网络(AutoEncoder)
- 深度玻尔兹曼机器(DBM)
- 卷积神经网络(CNN)
- 深度置信网(DBN)
- 递归神经网络(RNN)



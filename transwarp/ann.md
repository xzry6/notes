<head>
  <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
</head>
# Note: Artificil Neural Network
**人工神经网络**是受到了生物学上动物的中枢神经系统的启发而开发出来的一种计算模型.
**人工神经网络**最早于20世纪40年代提出, 但由于庞大的神经网络和当时计算能力的局限, 神经网络的研究一直停滞不前.
直到20世纪80年代**分散式并行处理**的流行和由*Paul Werbos*创造的反向传播算法, 神经网络渐渐又开始流行起来. 并于21世纪开始同**深度学习**一起成为机器学习领域的热点.

### 概要
* [结构](#结构)
* [前向传播](#前向传播)
* [反向传播](#反向传播)
* [主流模型](#主流模型)

### 结构
<img src="http://cs231n.github.io/assets/nn1/neural_net2.jpeg" height="240">

### 前向传播
<img src="http://ufldl.stanford.edu/tutorial/images/Network331.png" height="360">

#### 组合函数
$p(x_j^2) = \sigma(\Sigma w_{ij}^{12}x_i^1)$

$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$

$e^{i \pi} + 1 = 0$

<img src="http://ufldl.stanford.edu/tutorial/images/SingleNeuron.png" height="240">

#### 激活函数
- Sigmoid: \\( 1/x^{2} \\)
- 

- Tanh: h(z) = sinh(z) / cosh(z) = (e^z - e^-z) / (e^z + e^-z)
- ReLU: h(z) = max(0, z)

### 反向传播
<img src="http://www.forkosh.com/mathtex.cgi? \Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}">

![equation](http://www.sciweavers.org/tex2img.php?eq=1%2Bsin%28mc%5E2%29&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=)

### 主流模型
- 编码网络(AutoEncoder)
- 深度玻尔兹曼机器(DBM)
- 卷积神经网络(CNN)
- 深度置信网(DBN)
- 递归神经网络(RNN)



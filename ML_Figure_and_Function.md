# 【万字长文】整理一份全套的机器学习资料！

石晓文 [深度学习自然语言处理](javascript:void(0);) _1周前_

授权 | 小小挖掘机

作者 | 石晓文

本系列主要根据吴恩达老师的课程、李航老师的统计学习方法以及自己平时的学习资料整理！在本文章中，有些地方写的十分简略，不过详细的介绍我都附上了相应的博客链接，大家可以根据相应的博客链接学习更详细的内容。

本文的目录先列在这里啦：

#### 1、引言

#### 2、线性回归

#### 3、梯度下降法

##### 3.1 梯度下降法分类

##### 3.2 梯度下降法改进

#### 4、逻辑回归

#### 5、过拟合和正则化

##### 5.1 过拟合

##### 5.2 正则化

#### 6、方差vs偏差

##### 6.1 偏差(Bias)

##### 6.2 方差(Variance)

#### 7、支持向量机SVM

##### 7.1 最大间隔分类器

##### 7.2 超平面求解

##### 7.3 核函数Kernel

##### 7.4 Outliers

##### 7.5 SVM的损失函数

##### 7.6 LR和SVM的区别

#### 8、朴素贝叶斯

##### 8.1 贝叶斯定理

##### 8.2 朴素贝叶斯分类

#### 9、决策树方法

##### 9.1 特征选择

##### 9.2 决策树的生成

##### 9.3决策树剪枝

#### 10、集成学习方法

##### 10.1 Bagging

##### 10.2 Boosting

##### 10.3 Bagging与Boosting的异同

##### 10.4 Stacking

#### 11、总结一下机器学习中的损失函数

##### 11.1 0-1损失函数

##### 11.2 绝对值损失函数

##### 11.3 log对数损失函数

##### 11.4 平方损失函数

##### 11.5 指数损失函数

##### 11.6 Hinge损失函数

# 1、引言

#### 机器学习是什么？

Arthur Samuel：在进行特定编程的情况下，给予计算机学习能力的领域。
Tom Mitchell：一个程序被认为能从经验E中学习，解决任务T，达到性能度量值P，当且仅当，有了经验E后，经过P评判，程序在处理T时的性能有所提升。

#### 监督学习与无监督学习

根据训练数据是否有标记信息，机器学习任务大致分为两大类：监督学习和非监督学习，分类和回归是监督学习的代表，而聚类是非监督学习的代表。

#### 输入空间、特征空间、输出空间、假设空间

输入空间：在监督学习中，将输入所有可能取值的集合称为输入空间。
特征空间：每个具体的输入是一个实例，通常由特征向量表示，所有特征向量存在的空间成为特征空间。有时输入空间和特征空间为相同的空间，有时为不同的空间，需要将实例从输入空间映射到输出空间。
输出空间：在监督学习中，将输出所有可能取值的集合称为输出空间。
假设空间：监督学习的目的在于学习一个由输入到输出的映射，这一映射由模型来表示。由输入空间到输出空间的映射的集合，称为假设空间。举个简单的例子，在一元线性回归中，假设空间即所有的直线y=ax+b组成的集合，我们的目标就是找到一条y=a'x+b'，使得损失最小。

#### 生成模型和判别模型

生成模型：生成模型由数据学习联合概率分布P(X,Y)，按后求出条件概率分布P(Y|X)，作为预测的模型。之所以被称为生成方法，是因为模型表示了给定输入X产生输出Y的关系。典型的模型有朴素贝叶斯方法和隐马尔可夫模型的。
判别模型：判别模型由数据直接学习决策函数f(X),或者条件概率分布P(Y|X)。判别方法关心的是对给定的输入X，应该预测什么样的输出Y。典型的判别模型包括k近邻算法，感知机，决策树，逻辑回归，支持向量机等。

# 2、线性回归

#### 模型表示

线性回归是最简单的机器学习模型，其假设输入和输出之间满足线性关系，假设我们想要通过房屋尺寸来预测房价，通过将收集到的数据绘制在二维坐标系中，我们总中拟合得到图中的直线：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKrlMVhH2k1jxf32mEhvIcK5UVjt0ro3vHYwt6OC2qz2jLticUnUJVR5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

以二元线性回归为例，其表达式如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKMFwEEJgsrJ5YQgU2NYMFoDbDqWtromU5XdVVHYeD1AR6qELnzYMBSw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

对具有n个变量的多元线性回归问题，其一般表达式为:

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKul1ib4X1IolzzACXxK0p8zBMicGs1639aWh3ERfe6QVUuKuxgRxsjkwA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 损失函数

线性回归的损失函数一般是平方损失函数（上标i代表第i个数据，下表i代表第i维特征）：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKY7Mby4leTULTxcdQwzRYnuLwoVYFdeNR7WgzxT64c3PdagUsc4RcCw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

有时候我们可能把这个式子想的比较想当然，其实我们可以从极大似然的角度推导出平方损失函数，这时我们会假设损失服从正态分布，具体参考文章：
https://www.jianshu.com/p/4d562f2c06b8

#### 梯度下降

梯度下降(Gradient Descent)是一个用来求函数最小值的算法，后面会有一张专门讲解梯度下降法及其改进算法，这里只是介绍一个基本的思想。
梯度下降背后的思想是：开始时我们随机选择一个参数的组合，计算代价函数，然后我们寻找下一个能让代价函数值下降最多的参数组合。我们持续这么做直到找到一个局部最小值（local minimum），因为我们并没有尝试完所有的参数组合，所以不能确定我们得到的局部最小值是否便是全局最小值（global minimum），选择不同的初始参数组合，可能会找到不同的局部最小值。

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKdwyDycNS4j44dhtKSCS2SpfPo14CuiaX4S6fN4mU0ibgibt3SfMKMhSew/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

具体的做法就是：每一次都同时让所有的参数减去学习速率乘以损失函数的导数。

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKn8xctKzhkqaiaDzkTibV7k3JNzpYSI9A92mI0eXt8Av7xYyRRREvy6uA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中α是学习率（learning rate），它决定了我们沿着能让代价函数下降程度最大的方向向下迈出的步子有多大。

除学习率外，使用梯度下降法时需要对特征进行标准化处理，以二元线性回归为例，如果两维的数值范围相差特别大，梯度下降算法需要非常多次的迭代才能收敛，解决的方法是尝试将所有特征的尺度都尽量缩放到-1到1之间：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKb6A3cl8vyoWH6JuawytTBKUSnRT0ibibwNj3ic5Ifw6HlglzeCYeYwfFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

# 3、梯度下降法

再进一步介绍算法之前，我们先来介绍一下梯度下降法的分类及对其的改进。

## 3.1 梯度下降法分类

梯度下降法可以分为下面三种：

批量梯度下降法（Batch Gradient Descent）：批量梯度下降法，是梯度下降法最常用的形式，具体做法也就是在更新参数时使用所有的样本来进行更新。

随机梯度下降法（Stochastic Gradient Descent）：求梯度时没有用所有的m个样本的数据，而是仅仅选取一个样本j来求梯度。

小批量梯度下降法（Mini-batch Gradient Descent）：小批量梯度下降法是批量梯度下降法和随机梯度下降法的折衷，也就是对于m个样本，我们采用x个样子来迭代。

三者到达极小值的路径大致可以表示为下图：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKMn0wmicxqzj03hVNkLM9fuyUzXJNgxh5KMYjxNs7lgooHyYc0nZACQw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中，蓝色为批量梯度下降法，绿色为小批量梯度下降法，紫色为随机梯度下降法。

## 3.2 梯度下降法改进

#### 指数加权平均数

在讲解一些常见的梯度下降改进算法之前，我们先来看一下指数加权平均数(Exponentially weighted averages)：指数加权平均数是对趋势的一种刻画，计算公式如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKOEVIiaBPceeX9gZn1F4400vaqYBSJAmdXRHwPH1SDqpBy8BBHu4SObA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在上式中，vt是t时刻的指数加权平均值，v0 = 0，θt是第t时刻的实际值。β是一个参数。简单的理解，β代表了vt刻画的是多少天的平均值，如果β太小，那么vt越接近于θt，如果β很大，那么vt就能代表一个更长时间的平均值。

大体上，vt大概是1/(1-β)天的平均值，即当β=0.1时，我们可以认为vt代表了近10天的平均值。β=0.02时，我们可以认为vt代表了近50天的平均值，如下图所示，红色表示的是β=0.1时vt的变化，绿色表示的是β=0.02时vt的变化：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKvm6WEuWNhZjGtan0rrw4NKUa7HlzjHNCQHYv5ia8hq2zCibBcwXhDbMQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在指数加权平均的过程中，还有一个很重要的概念，叫做偏差修正(Bias correction),可以看到，如果v0=0，那么v1 = β*θ1，特别地，如果β=0.1，那么
v1 = 0.1 * θ1，这样导致v1会特别小，需要进行一定程度的修正， 具体的修正方法是对vt除以( 1-β^t)，当t特别小的时候，可以起到较为明显的修正效果，但是当t变大时，分母接近于1，基本没有进行修正。

在机器学习中，在计算指数加权平均数的大部分时候，大家不在乎执行偏差修正，因为 大部分人宁愿熬过初始时期，拿到具有偏差的估测，然后继续计算下去。如果你关心初始时 期的偏差，在刚开始计算指数加权移动平均数的时

#### 动量梯度下降法(Momentum)

动量梯度下降法基本的想法就是计算梯度的指数加权平均数，并利用该梯度 更新你的权重。假设我们使用梯度下降法更新参数的路径如下图中蓝色的线所示：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK2iabhrURFRbAicfT7H7Tianb1QnF2JGbrvvx6xibUoZicYeUrImibvqBwZ0Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

上图中的路径上下波动减慢了梯度下降法的速度，如果我们用更大的学习率，可能导致这种波动进一步加大，变成图中的紫色的线。因此，你希望在纵轴上学习慢一点，消除这种上下的摆动，而在横轴上，你希望快速从左向右移动，移动到最小值处。

我们之前介绍的指数加权平均数，可以反映近一段时间的趋势，在纵轴上的上下波动，使得平均值接近于0，而在横轴方向的平均值仍然较大。利用这种思想，Momentum的计算过程如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK2YicYDDz2Obh0Je2K5QFRTRNFYSY85r2pveb6pdcxqg6bJZBB1xk0lA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### RMSprop(root mean square prop)

还是用上面的例子，我们假设纵轴代表参数b，横轴代表参数W：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKEiaN0EStx6IYCqqyJ61DvibHfEQ5xBYgVVPKUd4iajIe2aoXg0kVM8fTQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

所以，你想减缓𝑏方向的学习，即纵轴方向，同时加快，至少不是减缓横轴方向的学习， RMSprop 算法可以实现这一点，将趋势变成下面的绿色的线：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKZ8c8ZRry6Q54icGbXJnXWQwfiaTNU6ZMS1hxicaMzER6hEtNibtAhGtxOw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们这里仍然运用指数加权平均数，但并不是dW的平均数，而是(dW)^2的平均数，即：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKoLmSnSibTHl7uNPzYLRsRj7LGibHQNyFCMjvMFT7L8djBsXLprCkzEmw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在参数更新时：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK76GYtfCWQwhy5BqwtUgn0XWUVsJ12hP0GbGyjDyodaBXlZQRicD847w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

由于db较大，dw较小，因此SdW较小，Sdb较大，所以可以减小纵轴上的摆动，加速横轴上的学习速度。

#### Adam(Adaptive Moment Estimation)

Adam 优化算法基本上就是将 Momentum 和 RMSprop 结合在一起，它使用的是经过偏差修正后的指数加权平均数：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKFrE2Xez3GImiaodmicZtTjpL5R7OHtOG3icStBIYPWGGvAEcnryeKFIWg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK9usWSqr5cqtPTakJTKD0jxzD4z325QicpF7PQP5TWpRwb572jXqLlFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

最终的参数更新变为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKqhaRfibzFnH4ed07uXedEeT9ibVnm7N2rib4Idiau8h93MCudBk6n3q6SQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Adam 算法结合了 Momentum 和 RMSprop 梯度下降法，并且是一种极其常用的学习算法，被证明能有效适用于不同神经网络，适用于广泛的结构。

# 4、逻辑回归

#### 基本原理

逻辑回归被应用于分类问题，通过sigmoid函数对线性回归的预测值进行转换，使其在[0,1]的范围：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKBoImBnIgfIVibyTfMKvkXZKjanicKgIwqQakmhKlcGove45cLV5hhKyw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中，g被称为sigmoid函数，其形式如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKTIDhvQTy4d8qpLY22rKA8URiaGWRrlyIbVv5N8o1km7ZBCiaqdjkqiaXQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 损失函数

逻辑回归使用的损失函数并非平方损失函数，而是如下的形式：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKR5WZwRrgP4Licj5dAMj1zyfHLibXL0TchhIYZ0muqCulRMVjEH22VQSg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

主要的原因是，平方损失函数对于逻辑回归来说是非凸的，使用梯度下降法可能收敛到局部极小值，而上面的损失函数是凸函数，可以得到全局最小值：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKcQaabHQ9Pm2tibR6xFtGd3Kvp53mpuEwJzCTBbby0HWvW3YBrkC6ksg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 过程推导

逻辑回归的推导是面试中经常会问到的问题，通过梯度下降法进行推导时，我们用到的主要性质时sigmoid函数的导数性质：g'(x) = g(x)(1-g(x))，一定要牢记于心。下图是推导过程：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKTnGbBUs9edItOJezcbrflJ34hs8fzTm1KibJWUiaPo9pzQ027p5eIkxA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

可以记一下结论，到时候用于检验自己推导的对不对。对第j个参数求梯度，结果为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK0vk6gRKL9XP7mWPu7ZM4t27lMNcERXTb2ia37mjL13w0BichTT9oiaeUA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

逻辑回归为什么会选择sigmoid函数呢，总结有以下几方面吧:
1、函数连续，单调递增
2、求导方便
3、输出范围为(0,1)，可以用作输出层，结果可以表示概率
4、抑制两头，对中间细微变化敏感，对分类有利。

#### 解决多分类问题

逻辑回归还可以用于多分类，我们常用的策略被称为one vs all。

假设我们有三个类，如下图所示：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK8J5fgddEGPCPKtvJYRBXfxMAPZjtgJA4iaMRDg9f8ZvSicviavAe4icVnA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

采用one vs all策略时，有多少类别，我们就要训练几个分类器。每次选择一个类别作为正例，其他所有类别的样本作为负例，训练一个分类器：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKnRukiabqExBmmmrGfJfV1JXnAgfIb741Cp85Y0MF9EicTY6r8Ficia8scQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

最后，在我们需要做预测时，我们将所有的分类器都运行一遍，然后对每一个输入变量，都选择最高可能性的输出变量，即：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKUCKyxlibW8licOr7QfR1RFAaLEUmuz9ZCFgQc6GzQyHxwr839wISxyzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

# 5、过拟合和正则化

## 5.1 过拟合

机器学习的目的是使学到的模型不仅对已知的数据而且对未知的数据都能有很好的预测能力。当损失函数给定时，模型在训练集上的损失被称为训练误差，在测试集上的损失被称为测试误差。

如果模型不能很好的适应训练集，会造成欠拟合(underfit)。相反，如果模型过于强调拟合原始数据，会导致对未知数据拟合很差，这种情况被称为过拟合(overfit)。

看下面的例子：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK6grvSC8GyEHVCFJTs99LAjaUqj2Vxqz0nESJcmhibKiaZrmEJwPeDdhw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKYNHEPCJ7BKuDjIIkjiatDzpBMhLYRicbJT9s4gtbyiaQ2tPsWmh8jiaOlg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

随着模型的复杂度的提升，训练误差和测试误差往往呈现下面的趋势：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKpARdXHLSSAcRxgZP3utl1ouBs3BC8R3mSu2NNC0q3gzR2EdNNypITA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 5.2 正则化

正则化的目的是对过大的参数进行一定的惩罚，降低其对模型的影响，使我们能够得到一个较为简单的模型。这符合奥卡姆剃刀的原理，即在所有可能选择的模型中，能够很好地解释已知数据并且十分简单的才是最好的模型。

我们常用的正则化有L1正则化和L2正则化。

#### L1正则化

L1正则化即在损失函数的基础上增加参数的1范数：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKia33eObHv8XbdKialVTwZsW7o98nH33OorIQ76fslmb50Q7GR3AIVHiaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们常说，L1正则化具有参数选择的作用，直观从图像理解，如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKdiavhARg1anTVdliakVv48M5ARSniaeedJEj0NTa5xECIcJ7YTMuiclyiaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

当然，也可以通过数学推导得到结论，具体参考文章：https://www.jianshu.com/p/2f60e672d4f0

从贝叶斯角度看，当参数的先验概率符合拉普拉斯分布时，最大化后验概率可以得到添加1范数的损失函数。

#### L2正则化

L2正则化即在损失函数的基础上增加参数的2范数：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKkicbd6Vyx0RibFFLsNMqI2MCiagCVDLKm4w4ePa6kGONqBrcLibHIcIQEA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

L2正则化具有权重衰减的功能，图像如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK4ybvIJwJOiaec8CyXKu4cvMO3Do4CVAUsCibmy6L0bATbwLRibxJQUQCQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

从贝叶斯角度看，当参数的先验概率符合高斯分布时，最大化后验概率可以得到添加2范数的损失函数。

从贝叶斯角度看L1正则化和L2正则化，参考文章：https://www.jianshu.com/p/4d562f2c06b8

# 6、方差vs偏差

当你运行一个学习算法时，如果这个算法的表现不理想，那么多半是出现两种情况：要么是偏差比较大，要么是方差比较大。换句话说，出现的情况要么是欠拟合，要么是过拟合问题。那么这两种情况，哪个和偏差有关，哪个和方差有关，或者是不是和两个都有关？

## 6.1 偏差(Bias)

偏差基本对应于欠拟合问题，其表现是模型在训练集和验证集上的误差都比较大，随着数据集的增加，模型在训练集和验证集上的误差表现如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKjg6gZZb6Eo133r8yHYgPDjB3iakgIibgiagvkK3EaF8YoBrIK2SX77Vibg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在面临高偏差时，我们应该尝试以下的方法：尝试获得更多的特征，尝试增加多项式特征，尝试减少正则化程度λ。

## 6.2 方差(Variance)

方差问题对应于过拟合问题，其表现是模型在训练集上误差比较小，而在验证集上的误差远远高于训练集。另一个解释方差问题的角度是，对于同一个形式的模型(比如都是四次回归)，针对不同的训练集，其拟合得到的参数相差很大。随着数据集的增加，模型在训练集和验证集上的误差表现如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKRaSlL1HXCHhKWI5SQiclBhYpaMAJkKHshcjic8CATTyyS9NQrzOVKKzw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在面临高偏差时，我们应该采取下面的方法：获得更多的训练实例，尝试减少特征的数量，尝试增加正则化程度λ

# 7、支持向量机SVM

关于SVM的知识，可以参照pluskid的博客，写的真心赞！

博客链接：http://blog.pluskid.org/?page_id=683

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKianibdPS2LTjAzPSpc6iaIlkmDBic39Vz3TdjOibGLiaeq8dqPgGFIiafROeA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 7.1 最大间隔分类器

SVM的目标是找到一个最大间隔的分类器，使得两类数据点到超平面的最小距离最大：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKG1qBPUmiaH9sgyWia7WL4LactIUF0RbaG61Mwh7IpzgOXQeRyn75F3eg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

恰好在边界上的点被称为支撑向量Support Vector。

其目标函数为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKnYib2jSqoz20H4ACldaXRUope7L8NO8cdHjhfTqmao1u46z6pZNs4pA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中，约束条件在支撑向量处取得等号，非支撑向量取大于号。

## 7.2 超平面求解

利用拉格朗日乘子法，上面的目标函数和约束条件可以写为一个式子：

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们令：

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们分析一下上式，在a>=0的情况下，对于支撑向量，后面括号中的一项为0，因此对应的a可以取得大于0的值，而对于非支撑向量，后面括号中的一项大于0，此时对应的a必须等于0。因此，在所有约束条件都满足的情况下，我们可以得到θ(w)=1/2 * ||w||^2，因此原问题可以表示成：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKQrC3vVst3GjXp9zWaBRiadjJjHcDwWT1icXAwbica9bFK9rIIw6RDxvuA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

根据KKT条件，我们可以将上面的式子转换为对偶形式(关于KKT条件，参考博客：http://blog.pluskid.org/?p=702）：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK35GMicvkIyQwu7QRibDLmxT0sLfV3hAaSZRibMC6GSzSUGhsUSZWrkdyQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

因此，我们首先对w和b进行求导得到：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKvawa8clEjsBfEusMicdQ7z4hSpCwtg9BbFGJwwGriaiaqBsFDXgibZz3Jg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然后求解a，这里用到的是SMO算法，我们不再详细介绍。

求解得到w和b之后，我们可以得到超平面方程为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKtdTle0rpIunH9Y6zeic6Y6pgHxVdAruaBfHUTOjqfzdEFeh8ttqicWkA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

因此对于新点 x 的预测，只需要计算它与训练数据点的内积即可。更进一步，实际上只要针对少量的“支持向量”而不是所有的训练数据，因为我们前面介绍过，对于非支撑向量，其对应的a是等于0的。

## 7.3 核函数Kernel

前面介绍的是线性可分的情况，然而还有线性不可分以及非线性的情况。这里我们先来介绍非线性的情况，我们会介绍到一种核函数的方法。

如图中的数据，是一种典型的非线性情况，我们希望找到一个圆来区分两类数据：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK16JJmacXQ6EXDSIYdgeksibOPA8xel4TYZKFrKFEj5C7r2465AAxa2g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

因此超平面是类似下面的形式：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKul0UWQORvnv224D0kd41gwJdXoIHNjHgMmkGUzUlcaqnfl2EE0CpyQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

因此，我们可以将低维空间中的数据映射到高维空间中去，进而得到一个最大间隔的超平面。一般的，如果我们用 ϕ(⋅)表示这个映射，则超平面变为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKIiaWufsqvQwjcApp4icDOlcicHXrXWjFS9yxibRdYFC1eWMzmK6NXPm2TA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

但上面的方法会面临维数爆炸的问题，如果二维空间做映射，选择的新空间是原始空间的所有一阶和二阶的组合，得到了五个维度；如果原始空间是三维，那么我们会得到 19 维的新空间这个数目是呈爆炸性增长的。所以就需要核函数方法出马了。

使用核函数方法，可以在低维空间中快速计算两个向量在映射过后的空间中的内积，例如在二维空间中使用多项式核函数，可以快速得到五维空间中映射结果的内积(形式相同):

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK0jPvYDGTe4cCCzEL3XvHXkojNJCZNchTiaxOt7AToo1TapSleUM7wUg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

使用核函数，我们现在的分类超平面变为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKBYUfgsyrTU052XY2sSETTib7j13icC7bkAfhZ8EhNmwU7XBnd3GNicQicA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

常见的核函数有多项式核，高斯核，线性核：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKN1nxoNFExPq1pavT0cRAaU7UMlejqlib79hxKsG1CGAbZsuiciciaPBPFw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 7.4 Outliers

对于这种偏离正常位置很远的数据点，我们称之为outlier ，在我们原来的 SVM 模型里，outlier 的存在有可能造成很大的影响：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKdSGiaicic9nWaIZuGiabk4z2tKrYdpDdBHq4WGsvU3tYUtwbQFPp5I5kMw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

用黑圈圈起来的那个蓝点是一个outlier ，它偏离了自己原本所应该在的那个半空间，如果直接忽略掉它的话，原来的分隔超平面还是挺好的，但是由于这个 outlier 的出现，导致分隔超平面不得不被挤歪了，变成途中黑色虚线所示（这只是一个示意图，并没有严格计算精确坐标），同时分类间隔也相应变小了。当然，更严重的情况是，如果这个 outlier 再往右上移动一些距离的话，我们将无法构造出能将数据分开的超平面来。

为了处理这种情况，SVM 允许数据点在一定程度上偏离一下超平面。例如上图中，黑色实线所对应的距离，就是该 outlier 偏离的距离，如果把它移动回来，就刚好落在原来的超平面上，而不会使得超平面发生变形了。具体来说，原来的约束条件变为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKZkK4obJ7cDc36ia4S5qYD3GRscOMIaotG5kiauAzAVfJKey8CkUrtUibA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

而目标函数变为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKdZb78YKlv9JnZeib7z38lc4Q547PMZeiaMyXzJvpZrha60EjeicPK0nFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中 C 是一个参数，我们可以认为是对outlier的容忍程度，如果C越大，我们对outliers的容忍程度越小，反之越大。

完整的形式如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKP2mDsPMNF7k6u7Fx324jDGxEjuyVjB6MDEFGMwPq72gAf1ic4jkFqdg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 7.5 SVM的损失函数

SVM的损失函数被称为合页损失函数，如下图：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKNQUeg0p9MjPc6U1amytSAiapxkqdp4u15TsfbljUUhAz9SIPKOlHMGA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

当样本被正确分类且函数间隔大于1时，合页损失才是0，否则，就会产生一定的损失。

## 7.6 LR和SVM的区别

总结了几点LR与SVM的区别，和大家分享：
1、LR可以输出属于每一类别的概率，SVM则不行
2、LR是基于概率最大化推导的，而SVM是基于最大化几何间隔推导的
3、SVM的决策超平面只有少量的支撑向量决定，而LR所有的样本都参与决策面的更新，所以SVM对异常数据并不敏感，LR更加敏感
4、SVM依赖数据表达的距离测度，所以需要先对数据进行标准化处理，但是LR不需要。

# 8、朴素贝叶斯

朴素贝叶斯是基于贝叶斯定理与特征条件独立假设的分类方法。

## 8.1 贝叶斯定理

贝叶斯定理是关于随机事件A和B的条件概率的定理，形式如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKA2xDP7SVVCqDLgWIRAQlhotNA8x5rFRJqbNFPx6AIr5yic2Q223iaibrg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 8.2 朴素贝叶斯分类

朴素贝叶斯分类的基本思想是：给出待分类项，求解在此项出现的条件下其他各个类别的出现的概率，哪个概率较大就认为待分类项属于哪个类别，用贝叶斯定理表示为(这里的上标表示一维特征)：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKcMt5Vp03kkGPe87q0jpW2oAcBxzHPvsyg74HicicC25rrKsMXicAGrLbw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

分母对于所有的c都是相同的，因此可以省去，故有：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKGLzUrJ06HibnCo8cNria1fN4zpXQrRviafViaSxgXVIqwAcEacCiaiad0jNQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们要做的就是统计对每一个类别来说，每一维特征每个特征出现的频率。频率有可能出现0的情况，我们需要进行*拉普拉斯平滑操作。

拉普拉斯平滑：就是对每类别下所有划分的计数加1，这样如果训练样本集数量充分大时，并不会对结果产生影响，并且解决了频率为0的尴尬局面。

# 9、决策树方法

决策树（Decision Tree）是数据挖掘中一种基本的分类和回归方法，它呈树形结构，在分类问题中，表示基于特征对实例进行分类的过程，可以认为是if−then规则的集合，也可认为是定义在特征空间与类空间上的条件概率分布。下图是一个简单的决策树示例：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKmwzibAicxQk9yX1vkAalEoovWHg70ldV4onxqmAicDChhD1h7IsHrMkNA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

决策树模型的主要优点是模型具有可读性，分类速度快。在学习时，利用训练数据，根据损失函数最小化原则建立决策树模型；而在预测时，对新的数据，利用决策树模型进行分类。主要的决策树算法有ID3算法、C4.5算法和CART算法。

一个决策树的学习过程包括三个步骤：特征选择、决策树的生成以及决策树的修剪。

## 9.1 特征选择

熵：在信息论与概率统计中，熵表示随机变量不确定性的度量。设X是一个取有限个值得离散随机变量，其概率分布为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKicQXaxan3Z4o9MSqtLM3ILVCVA8OpZJuia9ibG0rAOrWu6ia1ianlzKrt8Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

则随机变量X的熵定义为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKxp4aYdhQ4kpNYvjpUbmz93bZOvJJs3AoJoCiar14Qq4iaOeUNNbn8aAw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

条件熵H(Y|X)表示在已知随机变量X的条件下随机变量Y的不确定性：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKgpX1Ubc7ia0ANXywNRDUibnVWicsakcjaqKELqpgwI74mTGUuW4mz9Qhw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

信息增益

信息增益表示得知特征X的信息而使得类Y的信息的不确定性减少的程度。信息增益大的特征具有更强的分类能力。特征A对训练数据集D的信息增益g(D,A)，定义为集合D的经验熵H(D)与特征A给定条件下D的经验条件熵H(D|A)之差，即：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKoFUC50C70uYLvIiajicgecjuhiavXqNhA2yiab3gzIKh6KHWo0yhFP1BWA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

一般地，熵H(Y)与条件熵H(Y|X)之差称为互信息。决策树学习中的信息增益等价于训练数据集中类与特征的互信息。

设训练数据集为D，|D|表示其样本容量，即样本个数。设有K
个类Ck,k=1,2,⋅⋅⋅,K,根据特征A的取值将D划分为n个子集D1,D2,⋅⋅⋅,Dn，记子集Di中属于类Ck的样本的集合为Dik。
则信息增益的计算过程如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKibibylZEk7eUdgibia6GgibUNUTnic54YhiacwgH8wNUFGaNicFLUZtkCnDlibA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

信息增益比

以信息增益作为划分训练数据集的特征，存在偏向于选择取值较多的特征的问题。使用信息增益比可以对这一问题进行校正。
信息增益比表示特征A对训练数据集D的信息增益比。gR(D,A)定义为其信息增益g(D,A)与训练数据集D关于特征A的值的熵HA(D)之比，即：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK0dRCkPwbaRkylHuZqnuXQPxaltcbMa5Y549lWWUmCYaBM4MGnWHhcw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

基尼系数
分类问题中，假设有K个类，样本点属于第k类的概率为pk，则概率分布的基尼系数定义为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKgfE2WFZaLqrsjVExSjNJSHEsz5dPDux5DQwY9hlUWt5ibwR4AfJ1sww/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

若样本集合D根据特征A是否取某一可能值a被分割成D1和D2
两部分，即：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKEX2jAa8yZNakef9JIofqic2RIJ939ORibsRLYO4GmJImdAzAWGQQqqaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

则在特征A的条件下，集合D的基尼指数定义为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK4PmibHvibTZktINCpI2hoU6zkZL6dKhgib0Sm9bKa7W9r5IG76pkBLpAQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

基尼系数Gini(D)表示集合D的不确定性，表示经A=a分割后集合D的不确定性。基尼系数越大，样本集合的不确定性越大，与熵类似。
从下图可以看出基尼指数和熵之半的曲线很接近，都可以近似地代表分类误差率。

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKBJQPkYdjsgSlibh19x1A6APXFH7xBC5oObpPhVw4hzDAhF6gDnq8atg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 9.2 决策树的生成

ID3算法
ID3算法的核心是在决策树各个结点上应用信息增益准则选择特征，递归地建构决策树。

具体的算法步骤如下图：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKfTPQQ1rFADsjzz9cYvJ7v31zh3dNbFRDMuxDMHuIopx7J2eQMvPkhg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

C4.5
与ID3算法相似，C4.5算法对ID3算法进行了改进，C4.5在生成的过程中，用信息增益比来选择特征

CART
分类树与回归树（classification and regression tree，CART）模型（Breiman）既可用于分类也可用于回归。

对分类树用基尼系数（Gini index）最小化准则，进行特征选择，生成二叉树，其具体步骤如下：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKq3Sx4eia8fXkJpsJTQ0O0Jd0GT1jPt2v6OPIH8vnjW81OziaGpeDX8Ng/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

接下来具体说说回归树是如何进行特征选择生成二叉回归树的。

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKAe7mT8XBgqvAAYNus0RO8sp93zkS2Stq5F8HYicHID4YVYTlT2SNReg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 9.3决策树剪枝

剪枝
决策树的过拟合指的是学习时过多地考虑如何提高对训练数据的正确分类，从而构建出过于复杂的决策树。解决过拟合的办法是考虑决策树的复杂度，对已生成的决策树进行简化，即剪枝（从已生成的树上裁剪调一些子树或叶结点，并将其根结点或父结点作为新的叶结点，从而简化分类树模型）。
下图展示了决策树剪枝的过程：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKicianVgedQZHXII0XPc7GTI9c356KNfxa11ichpSn0f8NKCqLZomkmEicw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

# 10、集成学习方法

集成学习的主要思想是利用一定的手段学习出多个分类器，而且这多个分类器要求是弱分类器，然后将多个分类器进行组合公共预测。核心思想就是如何训练处多个弱分类器以及如何将这些弱分类器进行组合。关于集成学习，大致可以分为三类：Bagging、Boosting和Stacking。

本章我们只介绍集成学习方法的基本思想，而具体的实现方法大家可以根据链接自行学习。

## 10.1 Bagging

Bagging即套袋法，其算法过程如下：

1、从原始样本集中抽取训练集.每轮从原始样本集中使用Bootstraping的方法抽取n个训练样本（在训练集中，有些样本可能被多次抽取到，而有些样本可能一次都没有被抽中）.共进行k轮抽取，得到k个训练集.（k个训练集相互独立）
2、每次使用一个训练集得到一个模型，k个训练集共得到k个模型.（注：根据具体问题采用不同的分类或回归方法，如决策树、神经网络等）
3、对分类问题：将上步得到的k个模型采用投票的方式得到分类结果；对回归问题，计算上述模型的均值作为最后的结果.

我们常见的Bagging算法是随机森林算法，关于随机森林算法的细节，可以参考博客：https://www.jianshu.com/p/8f99592658f2

## 10.2 Boosting

Boosting是一族可将弱学习器提升为强学习器的算法。关于Boosting的两个核心问题：
1、在每一轮如何改变训练数据的权值或概率分布？
通过提高那些在前一轮被弱分类器分错样例的权值，减小前一轮分对样本的权值，而误分的样本在后续受到更多的关注.
2、通过什么方式来组合弱分类器？
通过加法模型将弱分类器进行线性组合，比如AdaBoost通过加权多数表决的方式，即增大错误率小的分类器的权值，同时减小错误率较大的分类器的权值。而提升树通过拟合残差的方式逐步减小残差，将每一步生成的模型叠加得到最终模型。

我们常见的Boosting算法有AdaBoost，梯度提升决策树GBDT，XgBoost以及LightGBM。大家可以根据下面的参考资料进行学习：

AdaBoost：https://www.jianshu.com/p/f2017cc696e6
GBDT：https://www.jianshu.com/p/c32af083be5b
Xgboost：原论文：https://arxiv.org/pdf/1603.02754v1.pdf
博客：https://blog.csdn.net/github_38414650/article/details/76061893
LightGBM：LightGBM 中文文档：http://lightgbm.apachecn.org/cn/latest/index.html

## 10.3 Bagging与Boosting的异同

总结了以下四点不同：

样本选择
Bagging：训练集是在原始集中有放回选取的，从原始集中选出的各轮训练集之间是独立的.
Boosting：每一轮的训练集不变，只是训练集中每个样例在分类器中的权重发生变化.而权值是根据上一轮的分类结果进行调整.

样例权重
Bagging：使用均匀取样，每个样例的权重相等
Boosting：根据错误率不断调整样例的权值，错误率越大则权重越大.

预测函数
Bagging：所有预测函数的权重相等.
Boosting：每个弱分类器都有相应的权重，对于分类误差小的分类器会有更大的权重.

并行计算
Bagging：各个预测函数可以并行生成
Boosting：各个预测函数只能顺序生成，因为后一个模型参数需要前一轮模型的结果.

## 10.4 Stacking

stacking 就是当用初始训练数据学习出若干个基学习器后，将这几个学习器的预测结果作为新的训练集，来学习一个新的学习器。

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfK8ctcInb5SLEMedaB4eWQJnpOWRWmMSSKhFqqDnJeH5r1ao6NoZfzlg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

具体的原理以及python的实现例子可以参考文章：https://www.jianshu.com/p/3d2bd58908d0

# 11、总结一下机器学习中的损失函数

## 11.1 0-1损失函数

0-1损失是指，预测值和目标值不相等为1，否则为0：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKibe30cHFtbozSff3pmD5ic7ZUV7FFM4C3miczYyxMd9ODQXxTP8afjEsw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 11.2 绝对值损失函数

绝对值损失函数为：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKdJpxgkE6wd1PJXDC7odKv4ngZuvqTIZeuvq8840VKobkgyky5deJhQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 11.3 log对数损失函数

逻辑回归的损失函数就是对数损失函数，log损失函数的标准形式：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKca4N6qT1QEBeIVic9BibwjGKIaWoFibiaPeibtIticf0RgBEw6dcvvVRrr8Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 11.4 平方损失函数

回归问题中经常使用平方损失函数：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKia2TlrsfnY5qcdhOqmXc9c0c70Sia2fR9mumhumpVwMTccfhh8f4mklQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 11.5 指数损失函数

AdaBoost就是一指数损失函数为损失函数的。 指数损失函数的标准形式：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKbGibQ2P6KxEjmGZJwYhibnFD8E8bvk6vcrLARHVVGvecq825hu7XwcRg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 11.6 Hinge损失函数

SVM中使用的是Hinge损失函数：

![](https://mmbiz.qpic.cn/mmbiz_png/jYWFficmyzX4FOUbWLl8VxL7IlQbyDmfKmy4pOeVz0lQdwIDicYtXsf83d4X5tzCdtrpEWpuoELJibMG9wZqG4IDQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

# 参考资料：

1、线性回归原理和实现基本认识：https://blog.csdn.net/lisi1129/article/details/68925799
2、从贝叶斯角度看L1及L2正则化：
https://www.jianshu.com/p/4d562f2c06b8
3、梯度下降（Gradient Descent）小结：https://www.cnblogs.com/pinard/p/5970503.html
4、L1正则化及推导：https://www.jianshu.com/p/2f60e672d4f0
5、支持向量机系列：http://blog.pluskid.org/?page_id=683
6、合页损失函数的理解：https://blog.csdn.net/lz_peter/article/details/79614556
7、李航统计学习方法——算法3朴素贝叶斯法：https://www.cnblogs.com/bethansy/p/7435740.html
8、机器学习-决策树：https://www.jianshu.com/p/8c4a3ef74589
9、集成学习系列(七)-Stacking原理及Python实现：https://www.jianshu.com/p/3d2bd58908d0
10、集成学习—boosting和bagging异同https://www.cnblogs.com/dudumiaomiao/p/6361777.html
11、集成学习系列(一)-随机森林算法：https://www.jianshu.com/p/8f99592658f2
12、集成学习系列(二)-AdaBoost算法原理：
https://www.jianshu.com/p/f2017cc696e6
13、集成学习系列(五)-GBDT(梯度提升决策树)：
https://www.jianshu.com/p/c32af083be5b
14、Xgboost论文：https://arxiv.org/pdf/1603.02754v1.pdf
15、通俗、有逻辑的写一篇说下Xgboost的原理：https://blog.csdn.net/github_38414650/article/details/76061893
16、LightGBM 中文文档：http://lightgbm.apachecn.org/cn/latest/index.html
17、机器学习总结（一）：常见的损失函数
https://blog.csdn.net/weixin_37933986/article/details/68488339



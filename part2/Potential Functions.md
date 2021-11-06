---
sort : 2
---

# Potential Functions

&emsp;&emsp;本章介绍了适用于更一般的构型空间的导航规划器，包括适用于多维空间和非欧几里得空间的。

&emsp;&emsp;<font color="#3FBF3F">势函数（potential function）</font>$$U:\mathbb{R}^m \rightarrow \mathbb{R}$$是可微的实值函数。势函数的值可以看作能量，势的梯度相应地可以看作力。<font color="#3FBF3F">梯度（gradient）</font>$$ \Delta U(q) = DU(q)^T = \left[ \frac{\partial U}{\partial q_1}(q), \ldots, \frac{\partial U}{\partial q_m}(q) \right]^T $$是指向$$U$$增长的局部最快方向的向量(更严密的定义见附录C.5)。利用梯度，可以定义<font color="#3FBF3F">向量场（vector filed）</font>，为流形上的每个点分配一个向量。当$$U$$是能量的时候，梯度向量场有如下性质：闭合回路的功为零。

&emsp;&emsp;势函数法指引机器人的方式如图4.1所示。

```warning
本章主要讨论一阶系统（i.e. 忽略动力学），所以把梯度看成速度向量而非力向量。
```

&emsp;&emsp;势函数可以看作地形，机器人在上面从高值态运动到低值态。机器人沿着势函数的负梯度下降，沿着这种路径的方法被称为<font color="#3FBF3F">梯度下降（gradient descent）</font>。


<!-- 蓝 -->
<font color="#3399ff"></font>
<!-- 绿 -->
<font color="#3FBF3F"></font>
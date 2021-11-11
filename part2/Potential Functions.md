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

<center>
    <figure>
        <img src="../../notebook\part2\images\fig4_1.JPG" width=300px>
        <figcaption>
            Fig 4.1 负电荷吸引，正电荷排斥，生成了一条绕开障碍物到达目标点的路径，由虚线表示
        </figcaption>
    </figure>
</center>

&emsp;&emsp;势函数可以看作地形，机器人在上面从高值态运动到低值态。机器人沿着势函数的负梯度下降，沿着这种路径的方法被称为<font color="#3FBF3F">梯度下降（gradient descent）</font>，i.e. $$ \dot{c}(t) = - \nabla U(c(t)) $$。

<center>
    <figure>
        <img src="../../notebook\part2\images\fig4_2.JPG" width=400px>
        <figcaption> Fig 4.2 临节点的不同类型：（上）函数的图像，（下）函数的梯度 </figcaption>
    </figure>
</center>

&emsp;&emsp;机器人会停在梯度为零的点$$q^*$$（$$\nabla U(q^*) = 0$$），这种点称为<font color="#3FBF3F">临界点</font>。该点的类型（最大值、最小值或鞍点，如图4.2所示）由二阶微分决定。对于实值函数，二阶微分为<font color="#3FBF3F">Hessian矩阵</font>。若Hessien在$$q^*$$处非奇异，则$$q^*$$处的临界点是<font color="#3FBF3F">非退化的</font>，表明该点是孤立的。Hessian正定对应局部极小值；负定对应局部极大值。一般来说，只考虑Hessian非奇异的势函数，i.e. 只含孤立临界点的势函数，这样的势函数没有平坦的部分。

```note
对于梯度下降法，无需计算Hessian，因为机器人一般只能在局部极小值点终止运动，而非局部极大值点或鞍点。只有局部极小值点才是稳定的，因为机器人在该点受到扰动后还是会回到这个极小值点。目标点应该落在局部极小值点。
```
<center>
    <figure>
        <img src="../../notebook\part2\images\fig4_3.JPG" width=450px>
        <figcaption> 
            Fig 4.3 (a)含有三个障碍物的圆形边界的构型空间；(b)能量面的势函数；
            (c)能量面等值线图；(d)势函数的梯度向量
        </figcaption>
    </figure>
</center>

&emsp;&emsp;有许多有效且可以在线计算的势函数，但是它们有一个**共通的问题：局部极小值的位置和目标不匹配**。也就是说，很多势函数无法得出完备的路径规划器。解决这个问题有两种思路：1.利用基于采样的规划器对势场进行增广；2.定义只有一个局部极小值点的势函数，被称为navigation function。尽管完备（或解析完备），这些方法都需要在规划前对构型空间有充分的了解。

&emsp;&emsp;本章讨论的算法都是可以用于任意维空间的，除非特殊说明。本章还对如何实现在平面上操作的移动底盘（i.e. 二维欧式构型空间内的一个点）。

<br />

## 1 Additive Attractive/Repulsive Potential

&emsp;&emsp;$$\mathcal{Q_{free}}$$中最简单的势函数就是attractive/repulsive potential。目标会吸引机器人，而障碍物会排斥它，它们共同作用引导机器人远离障碍到达目标点。势函数可以构造成二者之和$$ U(q) = U_{att}(q) + U_{rep}(q) $$。

### The Attractive Potential

&emsp;&emsp;势场$$U_{att}$$需要满足以下标准：1.$$U_{att}$$应关于与$$q_{goal}$$的距离单调增加。最简单的一种就是conic(圆锥曲线) potential，计算到目标的比例距离，i.e. $$ U(q) = \xi d(q,q_{goal}) $$。其中，$$\xi$$是用来缩放吸引势的作用的参数。吸引梯度为$$ \nabla U(q) \frac{\xi}{d(q,q_{goal})} (q-q_{goal}) $$。梯度向量按大小$$\xi$$指向远离目标的方向，在构型空间内除了目标点外所有点，目标点处没有定义。从非目标点处开始，沿着负梯度，有一条指向目标的路径。

&emsp;&emsp;选上面这种势函数在进行数值实现时，梯度下降会有很多问题，因为吸引势在原点是不连续的。因此，我们倾向于<font color="#3399ff">选择连续可微的势函数</font>，这样吸引势的大小随着机器人接近$$q_{goal}$$变小。这种势函数最简单的就是随与目标的距离二次增长的，e.g. $$ U_{att}(q) = \frac{1}{2} \xi d^2 (q,q_{goal}) $$，其梯度为$$ \nabla U_{att}(q) = \frac{1}{2} \xi \nabla d^2 (q,q_{goal}) = \xi (q,q_{goal}) $$ ，是从$$q$$开始指向远离$$q_{goal}$$方向的向量，大小与二者之间的距离成正比。也就是说，当机器人离目标较远时，速度较快，距离变进时，速度会变慢。

<center>
    <figure>
        <img src="../../notebook\part2\images\fig4_4.JPG" width=420px>
        <figcaption> 
            Fig 4.4 (a)吸引梯度向量场；(b)吸引等势线；(c)吸引势的图像
        </figcaption>
    </figure>
</center>

&emsp;&emsp;这种势函数当机器人离目标很远的时候会生成过大的速度，所以我们可以把这两种函数结合起来。

$$  U_{att}(q)
= \begin{cases}
    \frac{1}{2} \xi d^2 (q,q_{goal}) 
    & , \quad d(q,q_{goal}) \le d_{goal}^* \\
    d_{goal}^* \xi d(q,q_{goal}) - \frac{1}{2} \xi (d_{goal}^*)^2
    & , \quad d(q,q_{goal}) \gt d_{goal}^* 
\end{cases}
\tag{4.2}
$$

$$  \nabla U_{att}(q)
= \begin{cases}
    \xi (q-q_{goal}) & ,\quad d(q,q_{goal}) \le d_{goal}^* \\
    \frac{d_{goal}^* \xi (q-q_{goal})}{d(q,q_{goal})}
    & , \quad d(q,q_{goal}) \gt d_{goal}^* 
\end{cases}
\tag{4.3}
$$

其中，$$ d_{goal}^* $$是规划器在圆锥曲线和二次曲线之间切换时对应的与目标点的距离。

### The Repulsive Potential

<center>
    <figure>
        <img src="../../notebook\part2\images\fig4_5.JPG" width=300px>
        <figcaption> 
            Fig 4.5 排斥势只在障碍物的附近有效
        </figcaption>
    </figure>
</center>

&emsp;&emsp;排斥势会使机器人远离障碍物，排斥力的大小与机器人离障碍物的距离有关。因此，排斥势往往用与最近的障碍物的距离$$D(q)$$来定义

$$  U_{rep}(q) = 
\begin{cases}
  \frac{1}{2} \eta \left( \frac{1}{D(q)} - \frac{1}{\mathcal{Q}^*} \right)^2
  \quad , & D(q) \le \mathcal{Q}^* \\
  0 \quad , & D(q) \gt \mathcal{Q}^*
\end{cases}
\tag{4.4}
$$

其梯度为

$$  \nabla U_{rep}(q) =
\begin{cases}
  \eta \left( \frac{1}{\mathcal{Q}^*} - \frac{1}{D(q)} \right)
  \frac{1}{D^2(q)} \nabla D(q)
  \quad , & D(q) \le Q^* \\
  0 \quad , & D(q) \gt Q^*
\end{cases}
\tag{4.5}
$$

其中，$$ \mathcal{Q} \in \mathbb{R} $$使得机器人可以忽略距离足够远的障碍，$$\eta$$可以看作是排斥梯度的增益。这些标量通常通过试错和误差来决定。（如图4.5所示。）

<center>
    <figure>
        <img src="../../notebook\part2\images\fig4_6.JPG" width=300px>
        <figcaption> 
            Fig 4.6 距离为到障碍物上最近的点的距离。梯度是指向背离最近点方向的单位向量。
        </figcaption>
    </figure>
</center>

&emsp;&emsp;在对这个方法进行数值实现的时候，路径可能会在某个点周围振荡，因为它们与障碍的距离相等，i.e. 不平滑的$$D$$上的点。为了避免这种振荡，用单个障碍物的距离来重新定义排斥势函数，其中$$d_i(q)$$是与障碍$$\mathcal{QO_i}$$的距离，如

$$ d_i(q) = \min_{c \in \mathcal{QO}_i} d(q,c)
\tag{4.6}
$$

对于凸障碍物$$\mathcal{QO_i}$$，$$c$$是距离$$x$$最近的点，$$d_i(q)$$的梯度为

$$  \nabla d_i(q) = \frac{q-c}{d(q,c)}
\tag{4.7}
$$

向量$$ \nabla d_i(q) $$描述了能够最大地增加$$\mathcal{QO_i}$$和$$q$$之间距离的方向（如图4.6所示）。

&emsp;&emsp;这样，每个障碍物都有自己的势函数

$$  U_{rep_i}(q) =
\begin{cases}
  \frac{1}{2} \eta 
  \left( \frac{1}{d_i(q)} - \frac{1}{\mathcal{Q}_i^*} \right)
  \quad & , \text{if } d_i(q) \le \mathcal{Q}_i^* \\
  0 \quad & , \text{if } d_i(q) \gt \mathcal{Q}_i^* \\
\end{cases}
$$

其中，$$\mathcal{Q}_i^*$$定义了障碍$$\mathcal{QO_i}$$作用域的大小。那么$$ U_{rep}(q) = \sum_{i=1}{n} U_{rep_i}(q) $$。<font color="#3399ff">如果只有凸障碍或能分解成凸块的非凸障碍，就不会发生振荡，因为规划器在最近的点处不会发生突变。</font>

<br />

## 2 Gradient Descent

&emsp;&emsp;梯度下降法是解决优化问题的著名方法。梯度下降算法如algorithm 4所示。

<center>
    <figure>
        <img src="../../notebook\part2\images\alg4_1.JPG" width=400px>
    </figure>
</center>

&emsp;&emsp;$$q(i)$$表示第i次迭代后$$q$$的值，最终的路径由迭代序列$$ {q(0),q(1),\ldots,q(i)} $$构成。标量$$\alpha(i)$$的值决定了第i次迭代的步长。$$\alpha(i)$$的值不能过大，否则机器人会”跳进”障碍物；也不能过小，否则会花费过多时间。在运动规划问题中，$$\alpha(i)$$的取值通常是特设的或者根据经验选取的。在优化书籍中有很多系统的选取方法。最后一点，梯度为零的情况很难准确达到，一般用$$ \parallel \nabla U(q(i)) \lt \epsilon \parallel $$替代。

<br />

## 3 Computing Distance for Implementation in the Plane

&emsp;&emsp;本节讨论如何构造吸引/排斥势函数。如果机器人当前位置和目标位置均已知，那么吸引势函数很简单。难度主要在于排斥势函数的计算，因为需要计算与障碍物的距离。本节介绍了两种计算距离及对应势函数的方法。第一种方法利用基于传感器的移动机器人来实现，借用第2章中通过传感器推断距离的思想。第二种方法假设构型空间被离散成像素栅格，并在栅格上计算距离。

### 3.1 Mobile Robot Implementation

<center>
    <figure>
        <img src="../../notebook\part2\images\fig4_7.JPG" width=300px>
        <figcaption> 
            Fig 4.7 射线的局部极值决定了到临近障碍物的距离
        </figcaption>
    </figure>
</center>

&emsp;&emsp;迄今为止，所有的讨论对于能够定义距离的构型空间具有一般性。考虑如图4.7所示的平面移动机器人，它装有放射状分布的距离传感器。这些距离传感器给出第2章中定义的原始距离函数$$\rho$$的近似。然而$$D(q)$$对应初始距离函数$$\rho$$的全局最小值，$$d_i(q)$$对应$$\rho(q,\theta)$$的局部最小值。比如，传感器会面向它对应的障碍物的最近的点，所以这些传感器指向能最大程度地将机器人移到距离障碍物最近的地方的方向，i.e. $$-\nabla d_i(q)$$。距离梯度指向与之相反。当障碍被另一个部分闭塞时，障碍距离函数可能会发生错误。


<!-- 蓝 -->
<font color="#3399ff"></font>
<!-- 绿 -->
<font color="#3FBF3F"></font>
---
sort : 1
---

# Configuration Space

## 6 Parameterizations of SO(3)

&emsp;&emsp;从前面的内容可以看出，旋转矩阵$$ R \in SO(3) $$的9个元素$$ R_{ij} $$受6个条件约束，剩下3个旋转自由度。因此，我们希望可以用3个变量来对$$ SO(3) $$作局部参数化。欧拉角是一种常见的参数化，但是我们无法用欧拉角来对$$ SO(3) $$作全局参数化。

## 7 Example Configuration Spaces

&emsp;&emsp;<font color="#3399ff">设计运动规划器的时候，理解机器人构型空间的基础结构通常是十分重要的。</font>

* $$ S^1 \times S^1 \times \cdots \times S^1 (n \text{times}) = T^n $$，n维圆环面
* $$ S^1 \times S^1 \times \cdots \times S^1 (n \text{times}) \neq S^n $$，$$ \mathbb{R}^{n+1} $$中的n维球体
* $$ S^1 \times S^1 \times S^1 \neq SO(3) $$
* $$ SE(2) \neq \mathbb{R}^3 $$
* $$ SE(3) \neq \mathbb{R}^6 $$

&emsp;&emsp;有的时候流形是否是紧的很重要。流形$$ S^n $$，$$ T^n $$和$$ SO(n) $$都是紧的，它们互相直接相乘也是紧的。而流形$$ \mathbb{R}^n $$和$$ SE(n) $$不是紧的，因此$$ \mathbb{R}^n \times \mathcal{M} $$也不是紧的。

&emsp;&emsp;以上构型空间有一个重要的<font color="#3399ff">相同点</font>：当它们被装进atlas时，都是<font color="#3399ff">可微流形</font>。其中，

* $$ \mathbb{R}^1 $$和$$ SO(2) $$是一维流形
* $$ \mathbb{R}^2 $$，$$ S^2 $$和$$ T^2 $$是二维流形
* $$ \mathbb{R}^3 $$，$$ SE(2) $$和$$ SO(3) $$是三维流形
* $$ \mathbb{R}^6 $$，$$ T^6 $$和$$ SE(3) $$是六维流形

## Transforming Configuration and Velocity Representations

&emsp;&emsp;e.g. $$q$$表示机器人手臂的连接角度，$$x$$表示终端传感器在周围空间内作为刚体的构形。规划操作的时候，用$$x$$表示更方便，但是控制机器人手臂的时候用$$q$$变量更方便，所以需要在两种表达方式之间切换。然而$$\mathcal{Q}$$和$$\mathcal{M}$$往往不是同胚的，甚至两个空间的纬度都不相等。

&emsp;&emsp;定义<font color="#3FBF3F">foward kinematics map</font>$$\color{green}{\Phi : \mathcal{Q}} \rightarrow \mathcal{M}$$，以及<font color="#3FBF3F">inverse kinematics map</font>$$\color{green}{\Phi^{-1} : \mathcal{M}} \rightarrow \mathcal{Q}$$。


<!-- 蓝 -->
<font color="#3399ff"></font>
<!-- 绿 -->
<font color="#3FBF3F"></font>
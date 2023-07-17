---
sort: 1
---

# Bezier curve & B-spline

## 1 Bezier曲线

### 1.1背景及意义

&emsp;&emsp;参数$$n$$次曲线 

$$ p(t)=\sum_{i=0}^n{a_if_{i,n}(t)} \qquad 0\le t \le1 $$

其中，系数矢量$$ a_i(i=0,1,2,\ldots,n) $$顺序首尾相接，$$ a_0 $$到$$ a_n $$连成的折线称为控制多边形（Bezier多边形）。


&emsp;&emsp;Bezier基函数

$$ 
f_{i,n}= 
\begin{cases}
\ 1 &,i=0\\
  \frac{(-t)^i}{(i-1)^i} \frac{\text{d}^{i-1}}{\text{d}^{i-1}} [\frac{(1-t)^{n-1}-1}{t}] &
\end{cases}
$$

可以化成伯恩斯坦函数

$$ B_{i,n}=C_n^it^i(1-t)^{n-1}
=\frac{n!}{i!(n-i)!}t^i(1-t)^{n-1} 
\qquad (i=0,1,2,\ldots,n)$$

<br />

### 1.2 定义

&emsp;&emsp;针对Bezier曲线，给定空间$$ n+1 $$个点的位置矢量$$ P_i(i=0,1,2,\ldots,n) $$，则<font color="#00B050">Bezier曲线段的参数方程</font>表示如下：

$$ p(t)=\sum_{i=0}^n{P_i B_{i,n}(t)} \qquad t\in[0,1] $$

其中，$$ p_i(x_i,y_i,z_i),i=0,1,2,\ldots,n $$是控制多边形的$$ n+1 $$个顶点，即构成该曲线的特征多边形；$$ B_{i,n}(t) $$是<font color="#00B050">Bernstein基函数</font>，有如下形式：

$$ B_{i,n}=\frac{n!}{i!(n-i)!}t^i(1-t)^{n-i}
=C_n^it^i(1-t)^{n-i} 
\qquad (i=0,1,2,\ldots,n)$$

二项式定理（牛顿二项式定理），给出两数之和的整数次幂的恒等式，$$ \sum_{i=0}^n{B_{i,n}(t)} $$恰好为二项式$$ [t+(1-t)]^n $$的展开式
（注：当$$i=0,t=0$$时，$$t^i=1,i!=1$$，即：$$0^0=1,0!=1$$）

&emsp;&emsp;$$P_i$$代表空间中的很多点，把$$t$$代入可以得到平面或空间内的一个点，当$$t$$从0变化到1时，就可以得到空间内的一个图形，即Bezier曲线。

<br /> 

**一次Bezier曲线**

$$ p(t)=\sum_{i=0}^n{P_i B_{i,n}(t)} \qquad t\in[0,1] $$

当$$n=1$$时，有2个控制点$$p_0$$和$$p_1$$，Bezier多项式是一次多项式：

$$ p(t)=\sum_{i=0}^1{P_i B_{i,n}(t)}
=P_0B_{0,1}(t)+P_1B_{1,1}(t)$$

其中，

$$ B_{0,1}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{1!}{0!(1-0)!} t^0 (1-t)^{1-0} =(1-t) $$

$$ B_{1,1}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{1!}{1!(1-0)!} t^1 (1-t)^{1-1} =t $$

所以

$$ p(t)=\sum_{i=0}^n{P_i B_{i,1}(t)}
=P_0B_{0,1}(t)+P_1B_{1,1}(t)
=(1-t)P_0+tP_1$$

恰好为连接起点$$p_0$$和终点$$p_1$$的<font color="#3399ff">直线段</font>。

<br /> 

**二次Bezier曲线**

$$ p(t)=\sum_{i=0}^n{P_i B_{i,n}(t)} \qquad t\in[0,1] $$

当$$n=2$$时，有3个控制点$$p_0$$、$$p_1$$和$$p_2$$，Bezier多项式是二次多项式：

$$ p(t)
=\sum_{i=0}^2{P_i B_{i,2}(t)}
=P_0B_{0,2}(t)+P_1B_{1,2}(t)+P_2B_{2,2}(t)$$

其中，

$$ B_{0,2}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{2!}{0!(2-0)!} t^0 (1-t)^{2-0} =(1-t)^2 
$$

$$ B_{1,2}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{2!}{1!(2-1)!} t^1 (1-t)^{2-1} =2t(i-t) 
$$

$$ B_{2,2}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{2!}{1!(2-2)!} t^2 (1-t)^{2-2} =t^2 
$$

所以

$$ 
\begin{equation}\begin{split} 
p(t)&=\sum_{i=0}^2{P_i B_{i,2}(t)} \\
    &=(1-t)^2P_0+2t(1-t)P_1+t^2P_2 \\
    &=(P_2-2P_1+P_0)t^2+2(P_1-P_0)t+P_0 \\
\end{split}\end{equation}
$$

为<font color="#3399ff">抛物线</font>，其矩阵形式为：

$$ p(t)
=\begin{bmatrix}t^2 & t & 1 \end{bmatrix}
\cdot \begin{bmatrix}1 & -2 & 1 \\ 
                    -2 & 2 & 0 \\ 
                    1 & 0 & 0 \\ \end{bmatrix}
\cdot \begin{bmatrix}P_0 \\ 
                     P_1 \\ 
                     P_2 \\ \end{bmatrix}
$$

**三次Bezier曲线**

$$ p(t)=\sum_{i=0}^n{P_i B_{i,n}(t)} \qquad t\in[0,1] $$

当$$n=3$$时，有4个控制点$$p_0$$、$$p_1$$、$$p_2$$和$$p_3$$，Bezier多项式是三次多项式：

$$ p(t)
=\sum_{i=0}^3{P_i B_{i,3}(t)}
=P_0B_{0,3}(t)+P_1B_{1,3}(t)+P_2B_{2,3}(t)+P_3B_{3,3}(t)
$$

其中，

$$ B_{0,3}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{3!}{0!(3-0)!} t^0 (1-t)^{3-0} =(1-t)^3 
$$

$$ B_{1,3}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{3!}{1!(3-1)!} t^1 (1-t)^{3-1} =3t(i-t)^2 
$$

$$ B_{2,3}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{3!}{1!(3-2)!} t^2 (1-t)^{3-2} =3t^2(i-t) 
$$

$$ B_{3,3}(t)=\frac{n!}{i!(n-i)!} t^i (1-t)^{n-i}
=\frac{3!}{1!(3-3)!} t^3 (1-t)^{3-3} =t^3 
$$

所以

$$ 
\begin{equation}\begin{split} 
p(t)&=\sum_{i=0}^3{P_i B_{i,3}(t)} \\
    &=(1-t)^3 P_0 + 3t(1-t)^2 P_1 + 3t^2(1-t) P_1 + t^3 P_3 \\
    &=(P_3-3P_2+3P_1-P+0)t^3 +3(P_2-2P_1+P_0)t^2+ 3(P_1-P_0)t +P_0 \\
\end{split}\end{equation}
$$

其中，$$ B_{0,3}(t)$$、$$B_{1,3}(t)$$、$$ B_{2,3}(t)$$
、$$ B_{3,3}(t) $$为三次Bezier曲线的基函数，均为三次曲线，任何三次Bezier曲线都是这四条曲线的线性组合。

<font color="#3399ff">Bezier曲线不能对曲线形状进行局部控制，如果改变任一控制点位置，整个曲线都会受到影响。</font>

&emsp;&emsp;三次Bezier曲线的矩阵形式为：

$$ 
\begin{equation}\begin{split} 
p(t)& =\begin{bmatrix}t^3 & t^2 & t & 1 \end{bmatrix}
   \begin{bmatrix}-1 & 3 & -3 & 1 \\ 
                  3 & -6 & 3 & 0 \\ 
                  -3 & 3 & 0 & 0 \\
                  1 & 0 & 0 & 0 \\ \end{bmatrix}
   \begin{bmatrix}P_0 \\ 
                  P_1 \\ 
                  P_2 \\
                  P_3 \\ \end{bmatrix} 
   \qquad t\in[0,1]          \\
& =T\cdot M_{be}\cdot G_{be} \\
\end{split}\end{equation}
$$

其中，$$ M_{be} $$是三次Bezier曲线系数矩阵（常数），$$ G_{be} $$是4个控制点位置矢量。

<br />

### 1.3 性质

#### 1.3.1 Bernstein基函数的性质

$$ B_{i,n}
=C_n^it^i(1-t)^{n-1}
=\frac{n!}{i!(n-i)!}t^i(1-t)^{n-1} 
\qquad (i=0,1,2,\ldots,n)
$$

1.正性（非负性)

$$ B_{i,n}(t)=
\begin{cases}
=0 \quad &t=0,1 \\
\gt 0 \quad &t \in (0,1),i=1,2,\ldots,n-1 \\
\end{cases}
$$

2.权性  
&emsp;&emsp;基函数有$$n+1$$项，其和正好为1，即
$$ 
\sum_{i=0}^n{B_{i,n}(t)} \equiv 1 \quad t \in (0,1)
$$

3.端点性质

$$B_{i,n}(0)=
\begin{cases}
1 \quad &(i=0) \\
0 \quad &\text{otherwise} \\
\end{cases}
$$

$$B_{i,n}(1)=
\begin{cases}
1 \quad &(i=n) \\
0 \quad &\text{otherwise} \\
\end{cases}
$$

4.对称性

$$
B_{i,n}(t)=B_{n-i,n}(1-t)
$$

将n次Bezier曲线控制多边形顶点位置不变、次序颠倒，则曲线保持不变，但是走向相反。

5.<font color="#3399ff">递推性</font>

$$
B_{i,n}(t)=(1-t)B_{i,n-1}(t)+tB_{i-1,n-1}(t) 
\quad (i=0,1,\ldots,n)
$$

即n次Bernstein基函数可由两个n-1次Bernstein基函数线性组合而成。

6.导函数

$$
B_{i,n}'(t)=n[B_{i-1,n-1}(t)-B_{i,n-1}(t)] \quad (i=0,1,\ldots,n)
$$

7.最大值

&emsp;&emsp;在$$ t=\frac{i}{n} $$处取到最大值。

8.积分

$$ \int_{0}^1{B_{i,n}(t)}=\frac{1}{n+1}$$

9.降阶公式

$$
B_{i,n}(u)=(1-u)B_{i,n-1}(u)+uB_{i-1,n-1}(t)
$$

即一个n次Bernstein基函数能表示成两个n-1次基函数的线性和。

#### 1.3.2 Bezier曲线的性质

1.端点性质  
&emsp;&emsp;顶点$$ p_0 $$和$$ p_n $$分别位于实际曲线段的起点和终点上，把$$ t= $$0和1代入可以得到$$ p_0 $$和$$ p_n $$。

2.一阶导数

$$ p'(t)=n \sum_{i=1}^n{(p_i-p_{i-1})B_{i-1,n-1}(t)}$$

当$$ t=0 $$时，$$ p'(0)=n(p_1-p_0) $$  
当$$ t=1 $$时，$$ p'(1)=n(p_n-p_{n-1}) $$  

3.几何不变性

4.变差缩减性  
&emsp;&emsp;若Bezier曲线的特征多边形是一个平面图形，
则平面内任意直线与$$ p(t) $$的交点个数不多于该直线与其特征多边形的交点个数。此性质表明Bezier曲线比特征多边形的折线更光顺。

<br />

### 1.4 Bezier曲线生成算法

#### 1.4.1 根据定义

$$ p(t)=\sum_{i=0}^n{P_i B_{i,n}(t)} \qquad t\in[0,1] $$

$$ B_{i,n}=\frac{n!}{i!(n-i)!}t^i(1-t)^{n-i}
=C_n^it^i(1-t)^{n-i} 
\qquad (i=0,1,2,\ldots,n)$$

1.给出$$ C_n^i $$的递归计算式：

$$ C_n^i 
= \frac{n!}{i!(n-i)!}
= \frac{n-i+1}{i} C_n^{-1} 
\qquad n \ge i
$$

2.将$$ p(t)=\sum_{i=0}^n{P_i B_{i,n}(t)} $$表示成分量坐标形式：

$$ \begin{cases}
x(t)=\sum_{i=0}^n{x_i}B_{i,n}(t) \\
y(t)=\sum_{i=0}^n{y_i}B_{i,n}(t) \qquad t \in [0,1] \\
z(t)=\sum_{i=0}^n{z_i}B_{i,n}(t) \\
\end{cases}
$$

#### 1.4.2 递推算法(de Casteljau)

由n+1个控制点$$ P_i(i=0,1,\ldots,n) $$定义的n次Bezier曲线$$ P_0^n $$可被定义为分别由前后n个控制点定义的两条n-1次Bezier曲线$$ P_0^n-1 $$和$$ P_1^{n-1} $$的线性组合：

$$ P_0^n=(1-t)P_0^n-1 + t P_1^{n-1} \qquad t \in [0,1] $$

由此得到<font color="#00B050">Bezier曲线的递推计算公式</font>

$$ P_i^k= \begin{cases}
P_i & k=0 \\
(1-t)P_i^k-1 + t P_{i+i}^{k-1} \quad & k=1,2,\ldots,n,i=0,1,\ldots,n-k
\end{cases}
$$

利用递推公式，在给定参数下，求Bezier曲线上一点$$ P(t) $$非常有效

<br />

### 1.5 Bezier曲线的拼接及升降阶

#### 1.5.1 Bezier曲线的拼接

$$ p(t)=\sum_{i=0}^n{P_i B_{i,n}(t)} \qquad t\in[0,1] $$

$$ B_{i,n}=C_n^it^i(1-t)^{n-i} 
\qquad (i=0,1,2,\ldots,n)$$

&emsp;&emsp;增加特征多边形的顶点数会引起Bezier曲线次数的提高，高次多项式会导致计算困难，所以需要采用分段设计，将各段曲线连接起来，并在接合处保持一定的连续条件。
 
&emsp;&emsp;给定两条Bezier曲线$$ P(t) $$和$$ Q(t) $$，相应控制点为$$ P_i (i=0,1,\ldots,n)$$和$$ Q_i (i=0,1,\ldots,m)$$

(1)$$ G^0 $$连续，则：$$ P_n=Q_0 $$

(2)$$ G^0 $$连续，只要保证$$ P_{n-1}，P_n=Q_0,Q_1 $$三点共线即可（Bezier曲线的起点（终点）处的切线方向和特征多边形的第一条边（最后一条边）的走向一致）

#### 1.5.2 Bezier曲线的升阶与降阶

**升阶**：保证曲线的形状和方向保持不变，但是增加定义它的顶点个数，即提高该Bezier曲线的次数。

&emsp;&emsp;设给定原始控制顶点$$ p_0,p_1,p_n $$，定义一条n次Bezier曲线，增加一个顶点后，仍定义同一条曲线的新控制顶点为$$ p_0^*,p_1^*,p_n^* $$，则有

$$ \sum_{i=0}^n{C_n^i P_i t^i (1-t)^{n-i}} 
=\sum_{i=0}^{n+1}{C_{n+1}^i P_i^* t^i (1-t)^{n-i}} 
$$

$$ P_i^* C_{n+1}^i
 = P_i C_n^i + P_{n-1} C_n^{j-1}
$$

化简得：
$$ P_i^*
=\frac{i}{n+1} P_{i-1} + \left(1-\frac{i}{n+1}\right)P_i
\qquad (i=0,1,\ldots,n+1)
$$

升阶后的多边形更加靠近曲线，如果一直升阶下去，控制多边形收敛于这条曲线。


**降阶**：升阶的逆过程

&emsp;&emsp;假定$$ P_i $$是由$$ P_i^* $$升阶得到，则由升阶公式有：

$$ P_i = \frac{n-i}{n} P_i^* + \frac{i}{n}P_{i-1}^* $$

由该方程可以导出两个递推公式：

$$ P_i^*
=\frac{nP_i-iP_{i-1}^*}{n-i} 
\qquad (i=0,1,\ldots,n-1)
$$

$$ P_{i-1}^*
=\frac{nP_i-(n-i)P_i^*}{i} 
\qquad (i=0=n,n-1,\ldots,1)
$$

可利用以上两个式子进行降阶，但不精确。 

<br />

## 2 B样条曲线

### 2.1背景

&emsp;&emsp;Bezier曲线存在不足：  
（1）一旦确定了特征多边形的顶点数，也就确定了曲线的阶次；  
（2）Bezier曲线或曲面的拼接比较复杂  
（3）Bezier曲线或曲面不能作局部修改

&emsp;&emsp;<font color="#00B050">样条(spline)：分段连续多项式</font>

如何分段？  
&emsp;&emsp;现有n+1个点，每两点之间构造一条多项式，n+1个点有n个小区间，  
每个小区间构造一条三次多项式，变成了n段的三次多项式拼接在一起，段与段之间满足两次连续，这就是**三次样条**。


<br /> 

### 2.2 B样条的递推定义和性质

&emsp;&emsp;<font color="#00B050">B样条曲线</font>的数学表达式为：

$$ P(u)=\sum_{i=0}^n {P_i B_{i,k}(u)}
\qquad u\in [u_{k-1},u_{n+1}]
$$

其中，$$ P_i(i=0,1,\ldots,n) $$是控制多边形的顶点，$$ B_{i,k}(u) $$称为k阶（k-1次）<font color="#00B050">B样条基函数</font>，k可以是2到控制点个数n+1之间的任意整数。

&emsp;&emsp;<font color="#3399ff">对于Bezier曲线来说，阶次和次数是一样的；但对于B样条，阶数是次数加1。</font>

&emsp;&emsp;B样条基函数是一个称为节点矢量的非递减的参数u的序列所决定的k阶分段多项式，这个序列称为<font color="#00B050">节点向量</font>。

#### de Boor-Cox递推定义

约定：$$ 0/0=0 $$

$$ B_{i,1}(u)=\begin{cases}
1 \qquad & u_i \lt u_{i+1}    \\
0 \qquad & Otherwise
\end{cases} $$

$$ B_{i,k}(u)
=\frac{u-u_i}{u_{i+k-1}-u_i} B_{i,k-1}(u)
+\frac{u_{i+k}-u}{u_{i+k}-u_{i+1}} B_{i+1,k-1}(u)
$$

该递推公式表明：若确定第i个k阶B样条$$ B_{i,k}(u) $$，需要用到$$ u_i,u_{i+1},\ldots,u_{i+k} $$共k+1个节点，称区间$$ [u_i,u_{i+k}] $$为$$ B_{i,k}(u) $$的支撑区间。

<br /> 

### 2.3 B样条基函数定义区间及节点向量

#### 2.3.1 k阶B样条对应节点向量数

$$ B_{i,1}(u)=\begin{cases}
1 \quad & u_i \lt u_{i+1}    \\
0 \quad & Otherwise
\end{cases} 
\qquad
 B_{i,k}(u)
=\frac{u-u_i}{u_{i+k-1}-u_i} B_{i,k-1}(u)
+\frac{u_{i+k}-u}{u_{i+k}-u_{i+1}} B_{i+1,k-1}(u)
$$

对于$$ B_{i,k} $$（k阶k-1次基函数）来说，涉及k个区间、k+1个节点。

#### 2.3.2 B样条函数定义区间

$$ P(u)=\sum_{i=0}^n {P_i B_{i,k}(u)}
\qquad u\in [u_{k-1},u_{n+1}]
$$

<font color="#00B050">“阶数+顶点”=节点向量的个数</font>

区间要合法，区间里必须要有足够的基函数与顶点匹配，B样条基函数严重依赖于节点向量的分布。

<br />

### 2.4 B样条曲线定义

$$ P(u)=\sum_{i=0}^n {P_i B_{i,k}(u)}
\qquad u\in [u_{k-1},u_{n+1}]
$$

$$ B_{i,1}(u)=\begin{cases}
1 \quad & u_i \lt u_{i+1}    \\
0 \quad & Otherwise
\end{cases} 
\qquad
 B_{i,k}(u)
=\frac{u-u_i}{u_{i+k-1}-u_i} B_{i,k-1}(u)
+\frac{u_{i+k}-u}{u_{i+k}-u_{i+1}} B_{i+1,k-1}(u)
$$

其中，$$ U_i $$是节点值，$$ U=(u_0,u_1,\ldots,u_{n+k}) $$构成了k阶（k-1）次B样条函数的节点矢量，B样条曲线对应的节点向量区间：
$$ u\in [u_{k-1},u_{n+1}] $$

<br />

### 2.5 B样条基函数的主要性质

1.局部支撑性

$$ B_{i,k}(u)\begin{cases}
\ge 0 \quad & u_i \lt u_{i+k} \\
=0    \quad & otherwise       \\
\end{cases} 
$$

对于每个区间$$ (u_i,u_{i+k}) $$，至多只有k个基函数在该区间内非零。

2.权性

$$ \sum_{i=0}^n{B_{i,k}(u)} \equiv 1 \qquad u \in [u_{k-1},u_{n+1}] $$

3.连续性

&emsp;&emsp;$$ B_{i，k}(u) $$在r重节点处的连续阶不低于k-1-r

4.分段参数多项式

&emsp;&emsp;$$ B_{i,k}(u) $$在每个长度非零的区间$$ [u_i,u_{i+1}) $$上都是次数不高于k-1的多项式。

<br />

### 2.6 B样条函数的主要性质

1.局部性

&emsp;&emsp;k阶B样条曲线上的一点至多与k个控制顶点相关，与其它控制顶点均无关。移动曲线的第i个控制点$$ P_i $$，至多影响到定义在区间上的那部分曲线的形状，对其余部分不产生影响。

2.变差缩减性

3.几何不变性

4.凸包性

&emsp;&emsp;B样条曲线落在$$ P_i $$构成的凸包之中，其凸包区域小于或等于同一组控制顶点定义的Bezier曲线凸包区域。 

<br />

### 2.7 B样条曲线类型的划分

1.均匀B样条曲线(uniform B-spline curve)

&emsp;&emsp;当节点沿参数轴均匀等距分布，即$$ u_{i+1}-u_i =\text{常数} \gt 0 $$时，表示均匀B样条函数。  
&emsp;&emsp;均匀B样条的基函数呈周期性。即给定n和k，所有的基函数形状相同，每个后续基函数时前面基函数在新位置上的重复：

$$ B_{i,k}(u) 
= B_{i+1,k}(u+\Delta u) 
= B_{i+2,k}(u+2\Delta u) 
$$

其中，$$ \Delta u $$是相邻节点值的间距。

2.准均匀B样条曲线(Quasi-uniform B-spline curve)

&emsp;&emsp;与均匀B样条曲线的差别在于两端节点具有重复度k，这样的节点矢量定义了准均匀的B样条基，解决了均匀B样条曲线没有保留Bezier曲线端点几何性质的问题。

3.分段Bezier曲线(Piecewise Bezier Curve)

&emsp;&emsp;节点矢量中两端节点具有重复度k，所有内节点重复度为k-1，这样的节点矢量定义了分段的Bernstein基。  
&emsp;&emsp;用分段Bezier曲线表示B样条曲线后，各曲线段就具有了相对独立性，且可以采用Bezier曲线的简单有效算法，缺点是增加了定义曲线的数据、控制定点数及节点数。

4.非均匀B样条曲线(non-uniform B-spline curve)

&emsp;&emsp;当节点沿参数轴的分布不等距，即$$ u_{i+1}-u_i \ne \text{常数}$$时，表示非均匀B样条函数。


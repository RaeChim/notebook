---
sort: 3
---

# Btraj论文笔记

[Github仓库](https://github.com/HKUST-Aerial-Robotics/Btraj)&emsp;
[论文](https://ieeexplore.ieee.org/document/8462878)

---

# Online Safe Trajectory Generation For Quadrotors Using Fast Marching Method and Bernstein Basis Polynomial

## 1 INTRODUCTION

&emsp;&emsp;[前期工作](https://ieeexplore.ieee.org/abstract/document/7784290)：提取构型空间（Configuration Space, C-Space）中的自由空间（free space），并用一系列凸形（convex shape）来表示自由空间，将轨迹生成问题转化成利用**凸优化**把轨迹限制在凸形内。存在以下两个问题：  
（1）分段轨迹的时间分配；  
（2）如何有效地将整条轨迹约束在自由空间内，并且其导数在具有硬约束的可行空间内。

&emsp;&emsp;本文利用快速行进算法（**fast marching**）在基于欧几里得符号距离场(Euclidean signed distance field，**ESDF**)的速度场内搜索一条时间索引路径（time-indexed path）。这种方法的时间分配是根据环境内的障碍物强度自适应的，并且可以得到优于基准（benchmark）方法的轨迹。针对第二个问题，采用Bernstein基来取代传统的单项多项式基，也就是分段Bezier曲线，利用优化可以保证轨迹的安全性和高阶动力学可行性。

（1）一种基于快速行进算法的路径搜索方法，应用于基于ESDF的速度场，为轨迹优化提供了合理的时间分配。  
（2）一种轨迹优化框架，通过利用Bernstein基保证生成的曲线平滑、安全且动力学可行。  
（3）所提出的运动规划方法的实时实现与系统集成在一个四旋翼平台。展示了室内和室外未知复杂环境下的自主导航飞行。

<br />

## 2 RELATED WORK

&emsp;&emsp;运动规划模块在自主机器人系统中起着重要的作用，它建立在系统的顶层，用于协调机器人在复杂环境中的导航。运动规划问题大致可以分为前端可行路径查找和后端轨迹优化。

&emsp;&emsp;对于前端寻路，有基于**采样**的和基于**搜索**的。基于采样的方法中具有代表性的有快速搜索树（RRT）算法，此方法在构型空间中随机采样并且引导树向目标生长。渐近最优采样方法有PRM*、RRG和RRT*。在这些方法中，随着样本数的增加，解最终收敛到全局最优。基于搜索的方法将构型空间离散化，并将寻路问题转化为图搜索。图可以在三维空间或高阶状态空间中定义。最近的典型方法包括JPS、ARA*(Any-time Repairing A*) 和hybrid A*。一般来说，由于机器人的动力学约束和路径本身的不平滑，前端生成的路径无法由机器人直接执行。所以，后端优化需要用时间$$ t $$对路径进行参数化并生成平滑轨迹。

&emsp;&emsp;**时间参数化**（time parametrization）或称时间分配过程，在连续空间内主导着轨迹的参数化，被认为是导致轨迹优化次最优的主要原因之一。实现良好时间分配的一种方法是使用梯度下降迭代地优化分配的时间，但迭代生成轨迹成本过高，无法满足实时性。在[前期工作](https://ieeexplore.ieee.org/abstract/document/7784290)中，我们根据分割点之间的欧氏距离为每一段轨迹分配时间，并利用飞行通道重叠区域来调整时间分配。其他人也有将**启发式**思想用到时间分配中的。本文采用快速行进方法，将时间分配紧密地融入到路径搜索中，而不是基于独立的前端寻路找到的的离散路径进行时间分配。在地图的ESDF引起的速度场上采用快速行进方法，搜索到一条自然的关于时间参数化的、自适应于障碍物密度的路径。

&emsp;&emsp;**轨迹优化**：生成平滑轨迹。基于梯度的，如CHOMP：通过安全性和平滑性的惩罚项将轨迹优化问题转化成**非线性优化**问题。最小捕捉轨迹规划算法（minimum
snap trajectory generation algorithm）：用分段多项式表示轨迹，将轨迹优化问题转化成二次规划（quadratic
programming，**QP**）问题。针对环境雕刻一个由简单凸区域组成的**飞行通道**，将飞行轨迹限制在通道内。通过迭代增加违逆极值的约束并求解凸规划（iteratively adding
constraints on violated extremum and re-solve the convex
program），保证安全性和动力学可行性，但这种方法需要通过多次迭代得到可行解。另一种方法是沿着轨迹对时间实例进行采样并在其上添加附加约束。这将使问题变得非常复杂，因为随着四旋翼速度的增加，约束必须密集地增加。本文用Bernstein基表示分段曲线，利用Bernstein基的性质，直接对整个轨迹施加安全性和动力学可行性约束。

<br />

## 3 FAST MARCHING-BASED PATH SEARCHING

### 3.1 Fast Marching Method

&emsp;&emsp;FM算法作为水平集（<font color="#33cc00">Level Set</font>）算法的特例，是一种模拟波前传播的数值方法，广泛应用于图像处理、曲线演化、流体模拟和路径规划。

&emsp;&emsp;假设波前以速度$$ f $$沿其法线方向传播，当波从震源到达某一点时，FM会计算这个时间。假设传播速度$$ f \gt 0 $$，即波前仅向外传播，并且$$ f $$是时不变的且仅与空间内的位置相关，那么波前的传播可以用程函方程（<font color="#33cc00">Eikonal differential equation</font>）表示:

$$ |\nabla T(x)| = \frac{1}{f(x)} 
\tag{1} $$ 

其中，$$ T $$是到达时间的函数，$$ x $$表示位置，$$ f(x) $$描述不同位置$$ x $$的速度。式(1)的近似解可参考[4] J. A. Sethian, “Level set methods and fast marching methods,” Journal
of Computing and Information Technology, vol. 11, pp. 1–2, 2003.

&emsp;&emsp;对于路径搜索，可以为机器人定义一个速度地图来导航。在模拟从起始点开始传播的波后，可以得到图中各点的到达时间。通过沿到达时间的梯度下降的方向，回溯路径，得到时间最短的路径。这是在路径搜索中应用快速行进算法的主要思想。与其他基于势场的方法不同，快速行进方法没有局部最小值，并且具有完备性和一致性。理论上，它和A*的时间复杂度相同，为$$ O(N \log (N)) $$。

&emsp;&emsp;**传统的图搜索算法**用在离散空间时，比如Dijkstra和A*，会搜索出一条离散的路径，然后对其用时间$$ t $$进行参数化。这会**导致后端轨迹优化的次优性**，因为在只给定路径长度的情况下很难对路径进行合理的时间参数化。而**快速行进法搜索的路径本身就是用$$ t $$作为参数的**。路径在速度场的到达时间度量上是最优的，而非距离，路径上的每个节点都与从源点传播的虚拟波的首次到达时间相关联。


### 3.2 Fast Marching in Distance Field

&emsp;&emsp;假设四旋翼会在环境中障碍物密集时减速，因为需要频繁调整姿态来避障，同样也会在稀疏环境中快速移动。换言之，根据与障碍物的距离确定四旋翼的允许速度是合理的。因此ESDF是设计速度地图的一个理想的参考。本文采用一个移位的双曲正切函数$$ \tanh (x) $$作为速度函数：

$$ f(d)=\begin{cases}
    v_m \cdot (\tanh (d-e)+1)/2 ,& 0 \le d \\
    0   & d \lt 0
\end{cases}
\tag{2}
 $$

其中，$$ d $$是ESDF场中位置$$ x $$的距离，$$ e $$是欧拉数，$$ v_m $$是最大速度。

```
双曲正切函数$$ \tanh (x) = \frac{\sinh x}{\cosh x} = \frac{e^x-e^{-x}}{e^x+e^{-x}} $$
```

&emsp;&emsp;为了提高波前的传播速度，在FM中引入启发函数，减少扩展节点的数量。
由于FM是应用于速度场的，所以可以用到达目标的最短时间$$ h(x) = d^*(x)/v_m $$作为启发函数，其中$$ d^*(x)$$是从$$ x $$到目标点的欧氏距离。

### 3.3 Flight Corridor Generation

&emsp;&emsp;为后端优化建立飞行通道，需要最大程度地利用自由空间，因为对于轨迹生成来说求解空间和求最优解同样重要。本文选择初始化一个飞行通道并扩大它。对于路径上的每一个节点，可以通过查询ESDF获得其到最近的障碍物的距离。这样，在每个节点上可以得到一个球形安全空间，将飞行通道初始化为球体的内接立方体。然后，通过沿$$ x, y, z $$方向查询相邻栅格来扩大每个立方体，直到它可能拥有的最大自由体积。由于飞行通道被初始化为一系列密集的节点，一些节点共享相同的放大立方体，这不是必须的，但增加了后端的复杂性。所以，删去通道中重复的立方体。维护八叉树地图的数据结构会带来额外的代价，所以本文选择更通用的体素网格（<font color="#33cc00">voxel grid</font>）。

<br />

## 4 SAFE AND DYNAMICALLY FEASIBLE TRAJECTORY GENERATION

&emsp;&emsp;本文采用Bernstein基将轨迹表示成分段Bezier曲线，它是B样条曲线的一个特例。

### 4.1 Bernstein Basis Piecewise Trajectory Formulation

&emsp;&emsp;Bernstein基定义：

$$ 
b_n^i(t) =  \begin{pmatrix}
                n \\ i \\
            \end{pmatrix} 
            \cdot t^i \cdot (1-t)^{n-i}
\tag{3}
$$

其中，$$ n $$是基的度，$$ (n \quad i)^T $$是二项式系数。Bernstein基构成的多项式称为Bezier曲线，写作：

$$ B_j(t) 
= c_j^0 b_n^0(t) + c_j^1 b_n^1(t) + \ldots +c_j^n b_n^n(t)
= \sum_{i=0}^n c_j^i b_n^i(t)
\tag{4}
$$

其中，$$ [c_j^0,c_j^1,\ldots,c_j^n] $$记作$$ c_j $$是Bezier曲线第$$ j $$段控制点的集合。与以单项式为基的多项式相比，Bezier曲线有以下特殊性质：  
（1）端点插值性；  
（2）区间不变性：$$ t \in [0,1] $$;  
（3）凸包性；
（4）Hodograph性：$$ B(t) $$的导数曲线$$ B^{(1)}(t) $$被称作hodograph，它仍是Bezier曲线，控制点为$$ c_i^{(1)}(t) = n \cdot (c_{i+1} - c_i) $$，可以用低阶贝塞尔曲线的控制点线性表示。

&emsp;&emsp;实际上，对于**Bezier曲线，控制点可以看作是基的权重，而基也可以看作是控制点的权重**。由于Bezier曲线是定义在不变的区间$$ t \in [0,1] $$上的，所以需要引入缩放系数$$ s $$。$$ \mu $$（可取x,y,z）轴上的$$ m $$段分段Bernstein基轨迹可以写成

$$ f_{\mu}(t) = \begin{cases}
    s_1 \cdot \sum_{i=0}^n{c_{\mu 1}^i b_n^i(\frac{t-T_0}{s_1})} 
                                        \quad & t \in [T_0,T_1] \\
    s_2 \cdot \sum_{i=0}^n{c_{\mu 2}^i b_n^i(\frac{t-T_1}{s_2})} 
                                        \quad & t \in [T_1,T_1] \\
    \qquad \qquad \vdots \\
    s_m \cdot \sum_{i=0}^n{c_{\mu m}^i b_n^i(\frac{t-T_{m-1}}{s_m})} 
                                        \quad & t \in [T_{m-1},T_m] \\
\end{cases} 
\tag{5}
$$

其中，$$ c_{ji} $$是第$$ m $$段轨迹的第$$ i $$个控制点，$$ T_1,T_2,\ldots,T_m $$是各段的结束时间，总时间为$$ T=T_m-T_0 $$，$$ s_1,s_2,\ldots,s_m $$为各段Bezier曲线的缩放系数，将$$ [0,1] $$缩放到$$ [T_{i-1},T_i] $$。在实际应用中，给每段乘上一个比例因子可以**使优化过程具有更好的数值稳定性**。

&emsp;&emsp;代价函数为$$ k $$阶导数平方的积分，为了最小化加加速度（<font color="#33cc00">jerk</font>），$$ k $$取3。目标函数为：

$$ J = \sum_{\mu \in \{x,y,z\}} 
       \int_0^T \left( \frac{d^k f_{\mu}(t)}{dt^k} \right)^2 dt 
\tag{6}       
$$

是一个二次型$$ p^T Q_0 p $$。$$ p $$是包含了所有控制点$$ c_{ij} $$的向量，$$ Q_0 $$是目标函数的Hessian矩阵。假设轨迹的第$$ j $$段在$$ \mu $$维的表达为$$ f_{\mu j}(t) $$，区间$$ [0,1] $$内无标度的Bezier曲线如式(4)所示。令$$ (t-T_j)/s_j = \tau $$，那么

$$ \begin{equation}\begin{split} 
J_{\mu j} 
& = \int_0^{s_j} \left( \frac{d^k f_{\mu j}(t)}{dt^k} \right)^2 dt \\
& = \int_0^1 s_j \left( \frac{s_j \cdot d^k (g_{\mu j}(\tau))}
                           {d(s_j \cdot \tau)^k} \right)^2 d\tau \\
& = \frac{1}{s_j^{2k-3}} \cdot 
  \int_0^1 \left( \frac{d^k g_{\mu j}(t)}
                       {d\tau ^k} \right)^2 d\tau 
\end{split}\end{equation}
\tag{7}
$$

仅由纬度$$ n $$和范围$$ s_j $$确定，并且可以写成典型的minimum snap公式。

### 4.2 Enforcing Constraints

&emsp;&emsp;为使分段轨迹平滑、安全且动力学可行，需要满足一些硬约束。Bezier曲线的高阶导数可以用对应的低阶控制点线性表示，如

$$ a_{\mu j}^{0,i} = c_{\mu j}^i,
a_{\mu j}^{l,i} = \frac{n!}{(n-l)!} \cdot (a_{\mu j}^{l-1,i+1}),
l \gt 1
\tag{8}
$$

其中，$$ l $$是导数的阶数，$$ n $$是Bernstein基的次数。

（1）**航点约束**：四旋翼需要通过的航点，如起点和目标点，及其$$ n $$阶导数，可以在相应控制点处设等式约束。对于第$$ j $$段轨迹起始处（航点）在$$ \mu $$维的$$ l $$阶导数（$$ l \lt n $$），有

$$ a_{\mu j}^{l,0} \cdot s_j^{(1-l)} = d_{\mu j}^{(l)}
\tag{9}
$$

（2）**连续性约束**：轨迹和所有的$$ \phi $$阶导数在连接处连续$$ (0 \le \phi \le k-1) $$。在两段相连轨迹的控制点处添加等式约束。对于第$$ j $$和$$ j+1 $$段曲线，有

$$ a_{\mu j}^{\phi,n} \cdot s_j^{(1-\phi)} 
 = a_{\mu ,j+1}^{\phi,0} \cdot s_{j+1}^{(1-\phi)} ,
a_{\mu j}^{0,i} = c_{\mu j}^i
\tag{10}
$$

（3）**安全性约束**：由于Bezier曲线具有凸包性，所以把控制点限制在3中生成的立方体内即可。对控制点添加边界条件，对于第$$ j $$段，有

$$ \beta_{\mu j}^- \le c_{\mu j}^i \le \beta_{\mu j}^+ , 
\mu \in \{ x,y,z \} ,i=0,1,2,\ldots,n
\tag{11}
$$

其中$$ \beta_{\mu j}^+ $$和$$ \beta_{\mu j}^- $$是第$$ j $$个立方体的上下界

（4）**动力学约束**：利用性质3和4，可以对整个轨迹的高阶导数施加硬约束。本文中速度范围为$$ [v_m^-,v_m^+] $$，加速度范围为$$ [a_m^-,a_m^+] $$。对第$$ j $$段的控制点有

$$ \begin{equation}\begin{split} 
& v_m^- \le n \cdot (c_{\mu j}^i - c_{\mu j}^{i-1}) \le v_m^+ , \\
& a_m^- \le n \cdot (n-1) \cdot (c_{\mu j}^i -2c_{\mu j}^{i-1} +c_{\mu j}^{i-2}) \le a_m^+ \\
\end{split}\end{equation}
$$

&emsp;&emsp;航点约束和安全约束直接表示为各路段（$$\mathbf {c}_j \in \Omega _j $$）优化变量的可行域。
将连续性约束和高阶动力学约束改写为线性等式约束
（$$ \mathbf{A}_{eq} \mathbf {c} = \mathbf{b}_{eq} $$)
和线性不等式约束
（$$ \mathbf{A}_{ie} \mathbf {c} \le \mathbf{b}_{ie} $$）
其中，$$ \mathbf{c} = [\mathbf{c}_1,\mathbf{c}_2,\ldots,\mathbf{c}_m] $$。轨迹生成问题可以化成：

$$ \begin{equation}\begin{split} 
\min \quad  & \mathbf{c}^T \mathbf{Q}_o c
\text{s.t.} \quad & \mathbf{A}_{eq} \mathbf{c} = \mathbf{b}_{eq}
                  & \mathbf{A}_{ie} \mathbf{c} \le  \mathbf{b}_{ie}
                  & \mathbf{c}_j \in \Omega _j , \quad j=1,2,\ldots,m
\end{split}\end{equation}
\tag{13}
$$

是一个凸二次规划问题（quadratic program,<font color="#33cc00">QP</font>），可以在多项式时间内由一般现成的凸优化器解决。

<br />

## 




$$  $$
<font color="#33cc00"></font>
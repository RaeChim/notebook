---
sort: 3
---

# FastLab论文笔记(1)

# Online Safe Trajectory Generation For Quadrotors Using Fast Marching Method and Bernstein Basis Polynomial

## INTRODUCTION

&emsp;&emsp;[前期工作](https://ieeexplore.ieee.org/abstract/document/7784290)：提取构形空间（Configuration Space, C-Space）中的自由空间（free space），并用一系列凸形（convex shape）来表示自由空间，将轨迹生成问题转化成利用**凸优化**把轨迹限制在凸形内。存在以下两个问题：  
（1）分段轨迹的时间分配；  
（2）如何有效地将整条轨迹约束在自由空间内，并且其导数在具有硬约束的可行空间内。

&emsp;&emsp;本文利用快速行进算法（**fast marching**）在基于欧几里得符号距离场(Euclidean signed distance field，**ESDF**)的速度场内搜索一条时间索引路径（time-indexed path）。这种方法的时间分配是根据环境内的障碍物强度自适应的，并且可以得到优于基准（benchmark）方法的轨迹。针对第二个问题，采用Bernstein基来取代传统的单项多项式基，也就是分段Bezier曲线，利用优化可以保证轨迹的安全性和高阶动力学可行性。

（1）一种基于快速行进算法的路径搜索方法，应用于基于ESDF的速度场，为轨迹优化提供了合理的时间分配。  
（2）一种轨迹优化框架，通过利用Bernstein基保证生成的曲线平滑、安全且动力学可行。  
（3）所提出的运动规划方法的实时实现与系统集成在一个四旋翼平台。展示了室内和室外未知复杂环境下的自主导航飞行。

<br />

## RELATED WORK

&emsp;&emsp;运动规划模块在自主机器人系统中起着重要的作用，它建立在系统的顶层，用于协调机器人在复杂环境中的导航。运动规划问题大致可以分为前端可行路径查找和后端轨迹优化。

&emsp;&emsp;对于前端寻路，有基于**采样**的和基于**搜索**的。基于采样的方法中具有代表性的有快速搜索树（RRT）算法，此方法在构型空间中随机采样并且引导树向目标生长。渐近最优采样方法有PRM*、RRG和RRT*。在这些方法中，随着样本数的增加，解最终收敛到全局最优。基于搜索的方法将构型空间离散化，并将寻路问题转化为图搜索。图可以在三维空间或高阶状态空间中定义。最近的典型方法包括JPS、ARA*(Any-time Repairing A*) 和hybrid A*。一般来说，由于机器人的动力学约束和路径本身的不平滑，前端生成的路径无法由机器人直接执行。所以，后端优化需要用时间$$ t $$对路径进行参数化并生成平滑轨迹。

&emsp;&emsp;**时间参数化**（time parametrization）或称时间分配过程，在连续空间内主导着轨迹的参数化，被认为是导致轨迹优化次最优的主要原因之一。实现良好时间分配的一种方法是使用梯度下降迭代地优化分配的时间，但迭代生成轨迹成本过高，无法满足实时性。在[前期工作](https://ieeexplore.ieee.org/abstract/document/7784290)中，我们根据分割点之间的欧氏距离为每一段轨迹分配时间，并利用飞行走廊重叠区域来调整时间分配。其他人也有将**启发式**思想用到时间分配中的。本文采用快速行进方法，将时间分配紧密地融入到路径搜索中，而不是基于独立的前端寻路找到的的离散路径进行时间分配。在地图的ESDF引起的速度场上采用快速行进方法，搜索到一条自然的关于时间参数化的、自适应于障碍物密度的路径。

&emsp;&emsp;**轨迹优化**：生成平滑轨迹。基于梯度的，如CHOMP：通过安全性和平滑性的惩罚项将轨迹优化问题转化成**非线性优化**问题。最小捕捉轨迹规划算法（minimum
snap trajectory generation algorithm）：用分段多项式表示轨迹，将轨迹优化问题转化成二次规划（quadratic
programming，**QP**）问题。针对环境雕刻一个由简单凸区域组成的**飞行走廊**，将飞行轨迹限制在走廊内。通过迭代增加违逆极值的约束并求解凸规划（iteratively adding
constraints on violated extremum and re-solve the convex
program），保证安全性和动力学可行性，但这种方法需要通过多次迭代得到可行解。另一种方法是沿着轨迹对时间实例进行采样并在其上添加附加约束。这将使问题变得非常复杂，因为随着四旋翼速度的增加，约束必须密集地增加。本文用Bernstein基表示分段曲线，利用Bernstein基的性质，直接对整个轨迹施加安全性和动力学可行性约束。

<br />

## FAST MARCHING-BASED PATH SEARCHING

### Fast Marching Method (FM)

&emsp;&emsp;FM算法作为水平集（<font color="#33cc00">Level Set</font>）算法的特例，是一种模拟波前传播的数值方法，广泛应用于图像处理、曲线演化、流体模拟和路径规划。

&emsp;&emsp;假设波前以速度$$ f $$沿其法线方向传播，当波从震源到达某一点时，FM会计算这个时间。假设传播速度$$ f \gt 0 $$，即波前仅向外传播，并且$$ f $$是时不变的且仅与空间内的位置相关，那么波前的传播可以用程函方程（<font color="#33cc00">Eikonal differential equation</font>）表示：  
$$ |\nabla T(x)| = \frac{1}{f(x)} 
\tag{1} $$  
其中，$$ T $$是到达时间的函数，$$ x $$表示位置，$$ f(x) $$描述不同位置$$ x $$的速度。式(1)的近似解可参考[4] J. A. Sethian, “Level set methods and fast marching methods,” Journal
of Computing and Information Technology, vol. 11, pp. 1–2, 2003.

&emsp;&emsp;对于路径搜索，可以为机器人定义一个速度地图来导航。在模拟从起始点开始传播的波后，可以得到图中各点的到达时间。通过沿到达时间的梯度下降的方向，回溯路径，得到时间最短的路径。这是在路径搜索中应用快速行进算法的主要思想。与其他基于势场的方法不同，快速行进方法没有局部最小值，并且具有完备性和一致性。理论上，它和A*的时间复杂度相同，为$$ O(N \log (N)) $$。

&emsp;&emsp;**传统的图搜索算法**用在离散空间时，比如Dijkstra和A*，会搜索出一条离散的路径，然后对其用时间$$ t $$进行参数化。这会**导致后端轨迹优化的次优性**，因为在只给定路径长度的情况下很难对路径进行合理的时间参数化。而**快速行进法搜索的路径本身就是用$$ t $$作为参数的**。路径在速度场的到达时间度量上是最优的，而非距离，路径上的每个节点都与从源点传播的虚拟波的首次到达时间相关联。




$$  $$
<font color="#33cc00"></font>
---
sort: 6
---

# Linear Programming

A Graduate Level Course

Shu-Cherng Fang

美国北卡莱罗纳大学方述诚教授在台湾国立交通大学的公开课的学习笔记。

<br />

---

### Course objective

&emsp;&emsp;本课程提供对线性优化**theory**和**algorithms**的基本理解，包含mathematical analysis，theorem proving，algorithm design和numerical methods。

```warning
本课程不涉及如何建立线性规划模型，该部分应该在introduction to operation research中。
本课程不涉及如何使用Excel Spreadsheets, SAS OPT, MATLAB, LINGO, CPLEX或其它求解器。
```

### Pre-requisites

&emsp;&emsp;1.Matrix Theory  
&emsp;&emsp;2.Linear Algebra  
&emsp;&emsp;3.Introduction to OR

### Course contents

&emsp;&emsp;1.Introduction to LP  
&emsp;&emsp;2.Geometric Interpretation of LP  
&emsp;&emsp;3.Simplex Method  
&emsp;&emsp;4.Duality and Sensitivity Analysis  
&emsp;&emsp;5.Interior Point Method  
&emsp;&emsp;6.Related Topics


## Textbook

Shu-Cherng Fang and Sarat Puthenpura, "Linear Optimization and Extensions: Theory and Algorithms." Prentice Hall 1993.

<br />

---

## Introduction

### What is Linear Programming (LP) ?

&emsp;&emsp;Optimize a **linear objective** function of decision variables subject to a set of **linear constraints**.

例

$$\begin{aligned}
    \text{Minimize} \quad & x_1 - 2 x_2 \\
    \text{subject to} \quad & x_1 + x_2 \le 40
    & 2 x_1 + x_2 \le 60 \\
    & x_1, x_2 \ge 0 .
\end{aligned}
$$

一般形式

$$\begin{aligned}
    \text{Minimize (Maximize)} \quad & \mathbf{c}^T \mathbf{x} \\
    \text{s.t.} \quad & \mathbf{Ax} = \mathbf{b} \\
    & \mathbf{x} \ge 0 .
\end{aligned}
$$

其中，$$ \mathbf{c} \in R^n $$，$$ \mathbf{b} \in R^m $$，$$ \mathbf{A} \in R^{m \times n} $$，约束关系也可以为不等式，如$$\mathbf{Ax} \ge \mathbf{bx}$$或$$\mathbf{Ax} \le \mathbf{bx}$$。

### Why Study Linear Programming ？

* 应用广泛
* 是以下课题的基础：

&emsp;&emsp;Nonlinear Programming 非线性规划  
&emsp;&emsp;Network Flows 网络流  
&emsp;&emsp;Integer Programming 整数规划  
&emsp;&emsp;Conic Programming 锥优化  
&emsp;&emsp;Semidefinite Programming 半正定优化  
&emsp;&emsp;Robust Optimization 鲁棒优化

参考：[Operations Research, Vol. 50, No. 1, 2002](https://pubsonline.informs.org/toc/opre/50/1)

例-人力分配

$$\begin{aligned}
    \text{Minimize} \quad & \sum_{i=1}^m \sum_{j=1}^n c_{ij} x_{ij} \\
    \text{s.t.} \quad   & \sum_{i=1}^m x_{ij} \ge b_j, j=1,\ldots,n \\
                        & \sum_{j=1}^n x_{ij} \le a_i, j=1,\ldots,m \\
                        & l_{ij} \le x_{ij} \le u_{ij}, \forall i,j .
\end{aligned}
$$

其中，$$x_{ij}$$为消耗在课题$$j$$上技能$$i$$的人力。

### How to Study Linear Programming ?

* Geometric intuition
* Algebraic manipulation
* Computer progrmming

### History of Linear Programming 

* **Conceived** by G.B. Dantzig (1947) —— mechanized planning tool for deployment, training an logistic supply program in USAF.
* **Named** by T.C. Koopmans & G. B. Dantzig (1948).
* **Simplex Method** proposed by G. B. Dantzig (1948).
* **Ellipsoid Method** proposed by L. G. Khachian (1979). —— First polynomial-time algorithm for LP.
* **Interior-Point Method** proposed by N. Karmarkar (1984). —— First "good" polynomial-time alorithm for LP.

### How to slove an LP problem ?

**What's special about LP ?**

——<font color="#3399ff">linear constraints</font>将可行域约束成具有有限个<font color="#3399ff">vertices</font>（顶点）的convex polyhedral set。

——<font color="#3399ff">linear objective</font>函数为每个定值提供一个<font color="#3399ff">linear contour</font>。

<font color="#3FBF3F">Fundamental Theorem of LP</font>：对于一个线性规划问题，如果可行域不为空，那么它的**optimum**是无限的或在可行域内至少有一个**顶点**。

**How to find an optimal solution ？**

**枚举法**——找到所有的顶点并比较选出最优的，但当$$ C(n,m) = \frac{n!}{m!(n-m)!} $$很大时不可行。

**Simplex Method**

&emsp;&emsp;基本思想：（沿边界行走）

* Step 1 ：从一个顶点开始
* Step 2 ：如果当前顶点最优则停止，否则
* Step 3 ：向更好的相邻顶点移动，返回Step 2。

**Is Simplex Method Good?**

&emsp;&emsp;（1）一般来说，单纯型法很好用。

&emsp;&emsp;（2）在最差的情况下，Klee和Minty (1971) 表明该方法需要遍历$$2^n-1$$个顶点。

&emsp;&emsp;（3）exponential-time algorithm问题

```tip
Polynomial Time Algorithm: 如果一个算法对任何实例I所采取的步数都被一个多项式约束在I的大小内。
Exponential-time algorithm: 如果一个算法不能在多项式时间内完成运算，那么它就是在指数时间内的。
```

**Is there a polynomial-time algorithm for LP?**

L. G. khachian于1979年提出了Ellipsoid Method。

&emsp;&emsp;基本思想

LP的解的特征为最优条件

$$  (*) \quad
    \left \{ 
        \begin{aligned}
            & \mathbf{c}^T \mathbf{x} - \mathbf{b}^T \mathbf{w} = 0 \\
            & \mathbf{Ax} \le \mathbf{b} ,\quad \mathbf{x} \ge 0 \\
            & \mathbf{A}^T \mathbf{w} \ge \mathbf{c} ,\quad \mathbf{w} \ge 0 \\
        \end{aligned}
    \right.
$$



&emsp;&emsp;
<br />
$$
$$
$$  $$
<!-- 蓝 -->
<font color="#3399ff"></font>
<!-- 绿 -->
<font color="#3FBF3F"></font>
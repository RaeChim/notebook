---
sort: 5
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
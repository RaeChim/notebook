---
sort: 3
---

# 整数规划

&emsp;&emsp;复旦大学管理学院孙小玲教授于2012年在台湾国立交通大学的公开课的学习笔记。课程的参考用书为孙小玲、李端的《整数规划》，课程资料孙教授放在了自己的[网站](http://my.gl.fudan.edu.cn/teacherhome/xlsun)上。很可惜这个网站我没能打开，可能是因为天妒英才，孙教授已经于2014年因病与世长辞，网站已无人维护。课程的讲义可以根据这个[CSDN文章](https://blog.csdn.net/weixin_40762182/article/details/124237403)获取，课程视频可以在[bilibili](https://www.bilibili.com/video/BV14z4y1Z7kw?spm_id_from=333.337.search-card.all.click&vd_source=ad1cdccd70e36fb8ead1355a256b8a4e)看到搬运的。

<br />
---

# Introduction

### What is Interger Programming?

* <b><font color="#00B050">Operations Research (O.R.)</font></b>是应用高级分析方法帮助做出更好决策的学科，即**Science of Better**。
* Mathematical Programming (Optimization)是运筹学的一个分支。它是关于如何从一组可用的备选方案中做出决策（符合某些标准）。
* <b><font color="#00B050">Integer Programming</font></b>是关于离散或整数变量的决策。

### Why bother to use integer decision variable?

* 如果变量和一个不可分的物理实体相关，那么它必须是整数，比如：要制造的飞机数量，股票数量……
* 如果要决定“是”或“否”（0或1）
* 在大多数情况下，离散决策的连续近似对于实际目的来说不够精确。

### Simple examples

**Example 1 (Stone Problem)**

<figure>
    <img src="./images/0/1.JPG" width=400px>
</figure>

**Example 2 背包问题 (Knapsack Problem)**

&emsp;&emsp;一个窃贼有一个大小为$$b$$的背包。他闯入一家商店，里面有$$n$$件物品。每件物品都有价值$$c_j$$和大小$$a_j$$。窃贼应该选择哪些物品来优化其盗窃行为？

&emsp;&emsp;令$$x_j = \begin{cases} 1,\ \text{选了第} j \text{个物品} \\ 0,\ \text{Otherwise} \end{cases}$$，问题可以建模为0-1整数规划模型：

$$\begin{array}{ll}
    \max & \sum_{j=1}^{n} c_{j} x_{j} \\
    \text { s.t. } & \sum_{j=1}^{n} a_{j} x_{j} \leq b \\
    & x_{j} \in\{0,1\}, j=1, \ldots, n
\end{array}$$

**Example 3 指派问题 (Assignment Problem)**

&emsp;&emsp;$$n$$个人可以做$$n$$项工作，每个人可以被指派去做一个工作，每个人$$i$$做工作$$j$$的成本为%%c_{ij}$$。问题是找到一个分配方法使成本最低。

&emsp;&emsp;令$$x_{ij} = \begin{cases} 1,\ \text{如果第} j \text{个人做工作}j \\ 0,\ \text{Otherwise} \end{cases}$$，问题可以表述为

$$\begin{array}{ll}
    \min & \sum_{i=1}^{n}\sum_{j=1}^{n} c_{ij} x_{ij} \\
    \text { s.t. } & \sum_{i=1}^{n} x_{ij}=1 ,\ i=1,\ldots,n \\
    & \sum_{i=1}^{n} x_{ij}=1 ,\ j=1,\ldots,n,\ x_{ij} \in \{0,1\}
\end{array}$$

#### Some Real-World Optimization Problems

* 火车调度
* 航空公司机组安排
* 公路路面养护与修复
* 生产计划
* 发电规划
* 电信
* 切割问题

### Classification of integer programming problems

* 我们称存在连续变量的问题为<b><font color="#00B050">混合整数规划</font></b>。
* 在有些问题中，$$x$$的取值只能是0或1，这一样的变量称为二元变量。含有二元变量的整数规划称为二元整数规划（BIPs，即<b><font color="#00B050">0-1规划</font></b>）。

&emsp;&emsp;<b><font color="#00B050">整数线性规划</font></b> Pure Integer Linear Programming：

$$\text{(IP)} \quad \min \{ 
        \mathbf{c}^T \mathbf{x} \mid \mathbf{Ax \leq b, x} \in \mathbb{Z}_+^n  
    \}
$$

&emsp;&emsp;<b><font color="#00B050">混合0-1规划</font></b> Mixed 0-1 Programming：

$$  \text{(MIP)} \quad \min \{
        \mathbf{c}^T \mathbf{x} + \mathbf{h}^T \mathbf{y} \mid 
        \mathbf{Ax + Gy \leq b}, \mathbf{x} \in \mathbb{B}^{n}, \mathbf{y} \in \mathbb{R}_{+}^{p}
    \}
$$

&emsp;&emsp;<b><font color="#00B050">非线性整数规划</font></b> General (Nonlinear) Integer Programming：

$$\begin{array}{lll}
    \text{(NLIP)} \quad & \min f(x) \\
    & \text { s.t. }  g_{i}(x) \leq b_{i}, i=1, \ldots, m, \\
    & \qquad x \in \mathcal{X},
\end{array}$$

其中$$f$$和$$g_i, i=1, \ldots, m$$为$$\mathbb{R}^n$$上的实值函数，$$\mathcal{X}$$是$$\mathbb{Z}^n$$（$$\mathbb{R}^n$$中所有整数点的集合）的一个有限子集。

&emsp;&emsp;<b><font color="#00B050">混合整数非线性规划</font></b> Mixed-Integer Nonlinear Programming：

$$\begin{array}{lll}
    \text{(MNLIP)} \quad & \min f(x, y) \\
    & \text { s.t. }  g_{i}(x, y) \leq b_{i}, i=1, \ldots, m, \\
    & \qquad x \in \mathcal{X}, y \in \mathcal{Y}
\end{array}$$

其中$$f$$和$$g_i, i=1, \ldots, m$$为$$\mathbb{R}^{n+q}$$上的实值函数，$$\mathcal{X}$$是$$\mathbb{Z}^n$$的一个有限子集，$$\mathcal{Y}$$是$$\mathbb{R}^q$$的一个连续子集。

### How Hard is Integer Programming?





## Course description
## Examples from combinatorial optimization

<br />
<!-- 蓝 -->
<b><font color="#3399ff"></font></b>
<!-- 绿 -->
<b><font color="#00B050"></font></b>
<!-- 橙 -->
<font color="#FF4500"></font>


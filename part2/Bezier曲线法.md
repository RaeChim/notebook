---
sort : 1
---

# Bezier曲线法

```note
对于车辆系统，轨迹应该满足以下准则：**轨迹连续；轨迹曲率连续；轨迹容易被车辆跟随，且容易生成**
```
&emsp;&emsp;对于$$ P_0,P_1,P_2,\ldots,P_n $$共n+1个控制点而言，Bezier点定义为

$$ p_n(t)
=\sum_{i=0}^n {C_n^i \cdot (1-t)^{n-i} \cdot t^i \cdot P_i}
=\sum_{i=0}^n{B_{i,n}(t) P_i} 
\qquad (i=0\text{时}，B_{i,n}=0)
$$

Bernstein基函数的一阶导数为：

$$ 
B_{i,n}'(t)
=[C_n^i (1-t)^{n-i} t^i]'
=n[B_{i-1,n-1}(t) - B_{i,n-1}(t)]
$$

则Bezier点求导为：

$$ p_n'(t)
=\left[ \sum_{i=0}^n {C_n^i (1-t)^{n-i} t^i P_i} \right ]'
=B_{0,n}' P_0 + B_{1,n}' P_1 + \cdots + B_{n,n}' P_n
=n \sum_{i=0}^n{B_{i-1,n-1}(t) (P_i - P_{i-1})} 
$$

```note
若要求曲率连续变化，则要求二阶导数连续，至少需要三阶Bezier曲线（四个控制点）才能生成曲率连续的路径。
```

 

$$  $$

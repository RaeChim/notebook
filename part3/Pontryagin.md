---
sort: 3
---

# Pontryagin Minimum Principle

&emsp;&emsp;考虑optimal control system

$$ \dot{\mathbf{x}}(t) = f(\mathbf{x}(t),\mathbf{u}(t),t) $$

和performance index

$$ J 
= S(\mathbf{x}(t_f),t_f)
+ \int_{t_0}^{t_f}V(\mathbf{x}(t),\mathbf{u}(t),t)dt
$$

以及给定boundary conditions

$$ \mathbf{x}(t=0)=\mathbf{x}_0 , 
\mathbf{x}(t=t_f)=\mathbf{x}_f \text{ is free, }
t_f  \text{ is free}
$$

现对控制$$ \mathbf{u}(t) $$添加约束条件如

$$ \|\mathbf{u}(t)\| \le \mathbf{U} $$

或其分量形式

$$ |u_j(t)| \le U_j 
\rightarrow U_j^- \le u_j(t) \le U_j^+
$$






$$  $$
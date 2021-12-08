---
sort: 3
---

# Appendix B

# State Space Analysis

## B.l State Space Form for Continuous-Time Systems

&emsp;&emsp;线性时不变（LTI）、连续、动态系统描述如下

$$\begin{aligned}
    \dot{\mathbf{x}}(t) = \mathbf{Ax}(t) + \mathbf{Bu}(t) &
    \qquad \text{state equation}    \\
    \mathbf{y}(t) = \mathbf{Cx}(t) + \mathbf{Du}(t)       &
    \qquad \text{output equation}
\end{aligned}   \tag{B.1.1}
$$

初始条件$$ \mathbf{x}(t=t_0) = \mathbf{x}(t_0) $$。

对上述方程组作Laplace变换，得

$$\begin{aligned}
    & \mathbf{X}(s) = [s\mathbf{I-A}]^{-1} [\mathbf{x}(t_0)+\mathbf{BU}(s)] \\
    & \mathbf{Y}(s) = \mathbf{C}[s\mathbf{I-A}]^{-1} [\mathbf{x}(t_0)+\mathbf{BU}(s)]
                      + \mathbf{DU}(s)
\end{aligned}   \tag{B.1.3}
$$

根据零初始条件$$ \mathbf{x}(t_0)=0 $$得传递函数$$ \mathbf{G}(s) $$，得

$$  \mathbf{G}(s)
  = \frac {\mathbf{Y}(s)} {\mathbf{U}(s)}
  = \mathbf{C} [s \mathbf{I} - \mathbf{A}]^{-1} \mathbf{B} + \mathbf{D}
$$

LTI系统的解为

$$\begin{aligned}
    & \mathbf{x}(t) = \pmb{\Phi}(t,t_0) \mathbf{x}(t_0)
                    + \int_{t_0}^t {\pmb{\Phi}(t,\tau) \mathbf{Bu}(\tau)} d \tau \\
    & \mathbf{y}(t) = \mathbf{C} \pmb{\Phi}(t,t_0) \mathbf{x}(t_0)
                    + \mathbf{C} 
                      \int_{t_0}^t {\pmb{\Phi}(t,\tau) \mathbf{Bu}(\tau)} d \tau
                      \mathbf{Du}(t)
\end{aligned}   \tag{B.1.6}
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
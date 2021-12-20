---
sort: 8
---

# Appendix B

# State Space Analysis

## B.l State Space Form for Continuous-Time Systems

&emsp;&emsp;<font color="#3FBF3F">线性时不变（LTI）、连续、动态</font>系统描述如下

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
                    + \mathbf{Du}(t)
\end{aligned}   \tag{B.1.6}
$$

其中，$$ \pmb{\Phi}(t,t_0) $$为系统的**状态转移阵**

$$  \pmb{\Phi}(t,t_0) = e^{\mathbf{A}(t-t_0)}
\tag{B.1.7}
$$

且有如下性质

$$	\pmb{\Phi}(t_0,t_0) = \mathbf{I} , \quad 
	\pmb{\Phi}(t_2,t_1)\pmb{\Phi}(t_1,t_0) = \pmb{\Phi}(t_2,t_0)
\tag{B.1.8}
$$

&emsp;&emsp;<font color="#3FBF3F">线性时变（LTV）、连续、动态</font>系统描述如下

$$\begin{aligned}
    \dot{\mathbf{x}}(t) = \mathbf{A}(t) \mathbf{x}(t) + \mathbf{B}(t) \mathbf{u}(t) &
    \qquad \text{state equation}    \\
    \mathbf{y}(t) = \mathbf{C}(t) \mathbf{x}(t) + \mathbf{D}(t) \mathbf{u}(t) &
    \qquad \text{output equation}
\end{aligned}   \tag{B.1.5}
$$

初始条件$$ \mathbf{x}(t=t_0) = \mathbf{x}(t_0) $$。

系统的解为

$$\begin{aligned}
    & 	\mathbf{x}(t) 
	= 	\pmb{\Phi}(t,t_0) \mathbf{x}(t_0)
      + \int_{t_0}^t {\pmb{\Phi}(t,\tau) \mathbf{B}(\tau) \mathbf{u}(\tau)} d \tau \\
    & 	\mathbf{y}(t) 
	= 	\mathbf{C}(t) \pmb{\Phi}(t,t_0) \mathbf{x}(t_0)
      + \mathbf{C}(t) 
	  	\int_{t_0}^t {\pmb{\Phi}(t,\tau) \mathbf{B}(\tau) \mathbf{u}(\tau)} d \tau
      + \mathbf{D}(t) \mathbf{u}(t)
\end{aligned}   \tag{B.1.9}
$$

其中，$$ \pmb{\Phi}(t,t_0) $$仍为系统的**状态转移阵**，难以计算，也有性质(B.1.8)。但是，根据基本矩阵$$ \mathbf{X}(t) $$满足

$$	\dot{\mathbf{X}}(t) = \mathbf{A}(t) \mathbf{X}(t)
\tag{B.1.10}
$$

可以写成

$$	\pmb{\Phi}(t,t_0) = \mathbf{X}(t) \mathbf{X}^{-1}(t_0)
\tag{B.1.11}
$$

<br />

## B.2 Linear Matrix Equations

&emsp;&emsp;关于已知矩阵$$\mathbf{A}$$和$$\mathbf{Q}$$的未知矩阵$$\mathbf{P}$$的线性联立方程组写作

$$  \mathbf{PA+A'P+Q} = 0
\tag{B.2.1}
$$

特别地，如果$$\mathbf{Q}$$正定，那么就存在一个唯一的正定阵$$\mathbf{P}$$满足方程，当且仅当$$\mathbf{A}$$是渐近稳定的或$$ \lambda \{\mathbf{A}\} $$的实部$$ （\text{Re}）\lt 0 $$。(B.2.1)被称为<font color="#3FBF3F">Lyapunov equation</font>，解为

$$  \mathbf{P} 
=   \int_0^{\infin} e^{\mathbf{A}'t} \mathbf{Q} e^{\mathbf{A}t} dt
\tag{B.2.2}
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
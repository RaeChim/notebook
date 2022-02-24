---
sort: 7
---

# 机器学习

试图入门机器学习的一些记录

---

## 1 Introduction

### 什么是机器学习？

"Learning denotes <font color="#3399ff">changes</font> in a system that enable a system to <font color="#3399ff">do the same task</font> <font color="#FF4500">more efficiently</font> the next time." -- Herbert Simon

"Learning is <font color="#3399ff">constructing or modifying representations</font> of what is <font color="#3399ff">being experienced</font>." -- Ryszard S. Michalski

Learning = <font color="#FF4500">improving</font> with <font color="#FF4500">experience</font> at same <font color="#FF4500">task</font> -- Tom M. Mitchell

<b><font color="#00B050">T (Task) &emsp;&emsp; E (Experience) &emsp;&emsp; P(Performance)</font></b>

```tip
Learning: change / construct or modify / improve
```

### 如何设计一个机器学习系统？

**Training Experience**

* What experience?  
注意<font color="#3399ff">training data bias: data, training procedure, features</font>  
* What exactly should be learned?
* How shall it be represented?
* What specific algorithm to learn it?


## 2 Preliminary: 基本概念

**给定：**  
&emsp;&emsp;<b><font color="#00B050">Instance Space</font></b> $$\color{green}{X}$$：可能的情况  
&emsp;&emsp;<b><font color="#00B050">Hypothesis Class</font></b> $$\color{green}{H}$$：假设  
&emsp;&emsp;<b><font color="#00B050">Training Examples</font></b> $$\color{green}{D}$$：positive an negative examples of the <b><font color="#00B050">Target Function</font></b> $$\color{green}{C}$$ $$<x_1,c(x_1)>,\ \ldots ,\ <x_m,c(x_m)>>$$

**决定：**  
&emsp;&emsp;A hypothesis $$h \in H$$ such that $$\color{green}{h(x) = c(x) ,\ \forall x \in X}$$，即找到一个假设使得对于所有的$$x \in X$$假设的输出都和实际的输出一样。

&emsp;&emsp;一般来说，$$X$$是无穷大或指数型的，所以一般来说无法保证对于所有的$$x \in X$$都有$$h(x) = c(x)$$。取而代之的是，选出一个好的近似，e.g. $$h(x) = c(x) ,\ \forall x \in D$$。



<br />

## 3 参考资料

[清华大学-机器学习概论](https://www.bilibili.com/video/BV17q4y1A7Vp?p=1)

[吴恩达]
---
sort: 3
---

# 强化学习纲要

&emsp;&emsp;本笔记主要参考[周博磊](https://boleizhou.github.io/)在香港中文大学开的[强化学习纲要](https://space.bilibili.com/511221970/channel/seriesdetail?sid=764099)一课，并结合了Richard S. Sutton和Andrew G. Barto的[《Reinforcement Learning: An Introduction》](http://incompleteideas.net/book/the-book-2nd.html)(2th edition)中的部分内容。

<style>
table
{
    margin: auto;
}
</style>

|            	  | Topic                                      	  | Resources |
|--------------	|----------------------------------------------	|----------	|
|  Lecture1 	| Overview (课程概括与RL基础)                                   	| Youtube([part1](https://www.youtube.com/watch?v=IkEF4LpH5Ys), [part2](https://www.youtube.com/watch?v=Qu8CPnnwplM)), B站([上集](https://www.bilibili.com/video/BV1LE411G7Xj/), [下集](https://www.bilibili.com/video/BV1g7411Z7SJ/))  |
|  Lecture2 	| Markov Decision Process (马尔科夫决策过程)                    	| Youtube([part1](https://www.youtube.com/watch?v=6yE9XiIB3hQ), [part2](https://www.youtube.com/watch?v=MIZbocCu7Sk)), B站([上集](https://www.bilibili.com/video/BV1g7411m7Ms/), [下集](https://www.bilibili.com/video/BV1u7411m7rh/)) |
|  Lecture3 	| Model-free Prediction and Control (无模型的预测和控制)          	| Youtube([part1](https://www.youtube.com/watch?v=Duj1U73yHik), [part2](https://www.youtube.com/watch?v=sfkhinBjGGY)), B站([上集](https://www.bilibili.com/video/BV1N7411Q7aJ/), [下集](https://www.bilibili.com/video/BV1N7411Q7M6/)) |
|  Lecture4 	| Value Function Approximation (价值函数近似)               	| Youtube([part1](https://www.youtube.com/watch?v=YdWsnB-u8PQ), [part2](https://www.youtube.com/watch?v=fGIaFlbBFxk)), B站([上集](https://www.bilibili.com/video/BV11V411f7bi/), [下集](https://www.bilibili.com/video/BV1w54y1d7se/))  |
|  Lecture5 	| Policy Optimization: Foundation (策略优化基础篇)             | Youtube([part1](https://www.youtube.com/watch?v=ProKaoyduFY), [part2](https://www.youtube.com/watch?v=MWXazkQkTlk)), B站([上集](https://www.bilibili.com/video/BV1fZ4y1x7mp/), [下集](https://www.bilibili.com/video/BV1ia4y1x7Va/))            	|
|  Lecture6 	| Policy Optimization: State of the art (策略优化进阶篇)      	| Youtube([part1](https://youtu.be/4YIdjLh-MJs), [part2](https://youtu.be/HOpiQWM0PCA)), B站([上集](https://www.bilibili.com/video/BV1s64y1M7AW/), [下集](https://www.bilibili.com/video/BV1EK41157fD/))  |
|  Lecture7 	| Model-based RL (基于环境模型的RL)                             	| [Youtube](https://youtu.be/2Cy8ZX16pBU), [B站](https://www.bilibili.com/video/BV1hV411d7Sg/)|
|  Lecture8 	| Imitation Learning (模仿学习)                         	| [Youtube](https://youtu.be/Sqvn6RxU8qk), [B站](https://www.bilibili.com/video/BV17k4y1k7Gu/)           	|
| Lecture9 	| Distributed systems for RL (分布式系统) 	| [Youtube](https://youtu.be/PyHGeFFfaWk), [B站](https://www.bilibili.com/video/BV1bi4y147Rv/)           	|
| Lecture10 	| RL in a nutshell (课程结局篇)| [Youtube](https://youtu.be/bDGmKVKAdHg), [B站](https://www.bilibili.com/video/BV1si4y1s7oQ/)           	|
| Bonus 1 	| DeepMind's AlphaStar Explained (剖析星际争霸AI) by [Zhenghao Peng](https://github.com/pengzhenghao)| [Youtube](https://youtu.be/5qp0VNC_iOc), [B站](https://www.bilibili.com/video/BV1wa4y1e74G/)           	|


&emsp;&emsp;需要注意的是，周老师的课程中使用的符号、公式等和Richard S. Sutton的书中不完全一样。另外，周老师的课虽然内容划分很清晰，对很多思想的解释也很形象，但是可能相对节奏较快，零基础的同学（比如本人）想要跟上有一定难度。[shuhuai008的白板推导系列课程](https://space.bilibili.com/97068901?spm_id_from=333.788.b_765f7570696e666f.1)可能更加适合新手，概念讲解和公式推导都很清晰，课程顺序、符号表示都是与参考书对应的，非常适合参照学习。目前，这门课程还在更新中（在我开始更新这份笔记的时候）。

&emsp;&emsp;关于深度强化学习部分，可以参考《Deep Reinforcemnet Learning Hands-on》一书。
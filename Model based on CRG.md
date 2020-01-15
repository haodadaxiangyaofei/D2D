#中国餐馆模型

##符号定义

| 符号                                                 | 含义                               | 对应中国餐馆模型                           |
| ---------------------------------------------------- | ---------------------------------- | ------------------------------------------ |
| $$l_i,i\in\{1,2,\dots,L\} $$                         | SVC layer i                        | tables                                     |
| $$u_i,i\in\{1,2,\dots,U\} $$                         | UE i                               | customer                                   |
| $$w_i, i\in \{1,2,\dots,U\},w_i\in \{1,2,\dots,L\}$$ | UE i所期望的最高layer              |                                            |
| $$v_i,i\in \{1,2,\dots,U\},v_i\in \{1,2,\dots,L\}$$  | UE i实际收到的最高layer            |                                            |
| $$\theta$$                                           | layer state                        | restaurant state                           |
| $$\{R_1(\theta),R_2(\theta),\dots,R_L(\theta)\}$$    | layer size function                | table size function                        |
| $$ R_i $$                                            | size of layer i                    | size of table i                            |
| $$x_i,i\in\{1,\dots,U\},x_i\in\{1,\dots,L\}$$        | UE i choose layer $$x_i$$ to relay | customer choose one table to sit           |
| $$n_{x_i}$$                                          | 选择layer $$x_i$$进行中继的UE数量  | no. of customer choose to sit on the table |

## UE的动作

* 首先BS广播各layer，根据UE和BS的距离决定UE能够收到的最高layer $$v_i$$
* 当UE还未获得自身所需的所有layer时，不进行中继服务。
* $$n_i$$所需的layer为：$$l_1,l_2,..., l_{w_i}$$,而$$u_i$$实际从BS收到的layer为：$$l_1,l_2,..., l_{v_i}$$，规定$$u_i$$从 $$l_{v_i+1}$$ 层开始逐层向上请求其他UE帮忙中继。
* 规定$$v_i \leq w_i$$，即UE不会为了提供中继服务而获取自己所不需要的layer。
* 当$$u_i$$的$$v_i = w_i$$时，$$u_i$$ 根据中国餐馆模型，选择某个layer进行售卖。
  * 假定任意UE均知道所有layer的状态（大小、提供中继的UE数）
  * layer i的大小$$R_i$$由需求layer i的UE数量决定
  * 之前UE做的决定可以存储在server的log file中以提供后续UE做决策[<sup>1</sup>](##参考)
  * $$u_i$$根据：$$R_i$$、之前UE做出的决策来进行选择

## 问题

* 具体的物理实现：
  * BS如何广播各layer
  * UE之间使用固定信道进行通讯，具体细节
  * UE作为中继获得的资源补偿如何对UE产生积极影响：UE成为中继时已经获取了所有自己需要的layer，额外的带宽补偿对UE有何益处
  * 有哪些物理资源需要考虑进来
* 模型的问题：
  * 目前[需求layer的人数]（桌子大小）、[提供layer的UE数]（桌子上的人数）、[之前UE做的选择]都是全局可知的信息，使模型变成简单的求平均桌子空间大小的问题。可能的解决办法：
    1. UE不光要考量当前的utility，同时要加入对后续UE决策的估计
    2. 将一部分信息变成局部的，UE需要根据signal和belief进行推测
  * 目前没有考虑向BS购买layer的情况
    1. BS会给出什么样的售价
    2. UE会给出什么样的售价

## 参考

[1]: Dynamic Chinese Restaurant Game: Theory and Application to Cognitive Radio Networks
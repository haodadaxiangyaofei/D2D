# SVC CRG D2D

## 符号定义

| 符号                                                 | 意义                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| $$l_i\in\{1,2,\dots,L\} $$                           | SVC layer i                                                  |
| $u_i\in\{1,2,\dots,U\} $                             | UE i                                                         |
| $$w_i, i\in \{1,2,\dots,U\},w_i\in \{1,2,\dots,L\}$$ | UE i所期望收到最高layer                                      |
| $$v_i,i\in \{1,2,\dots,U\},v_i\in \{1,2,\dots,L\}$$  | UE i实际收到的最高layer                                      |
| $$ R_i $$                                            | size of layer i，由需求该layer的UE数（已经拥有了layer 1~layer i-1，还需要layer i）决定 |
| $R_{i,j}$                                            | 在$u_i$的D2D通讯范围内需求layer j的UE数                      |
| $$x_i,i\in\{1,\dots,U\},x_i\in\{1,\dots,L\}$$        | UE i 提供 layer $$x_i$$ 的中继服务                           |
| $n_i$                                                | 提供layer i的中继服务的UE数量                                |
| $N = \{n_1,n_2,\dots,n_L\}$                          |                                                              |
| $n_{-i,j}$                                           | 除$u_i$外，其他选择提供layer j中继服务的UE的数量             |
| $N_{-i} = \{n_{-i,1},n_{-i,2},\dots,n_{-i,L }\}$     |                                                              |
| $U$                                                  | 可以售卖layer的UE的集合                                      |
| $U_i $                                               | UE i 的D2D范围内可以售卖layer的UE的集合                      |
| $\lambda(R_i,n_i)$                                   | 效用函数和layer的大小、选择该layer中继的UE数有关             |



## 过程

1. BS使用AMC进行广播，UE和BS的距离不同，收到的layer数也不同。下图是三圈、三层的例子

   <img src="https://s2.ax1x.com/2020/02/09/1flDVU.png" alt="1flDVU.png" style="zoom:67%;" />

2. UE之间通过D2D的方式来满足自身对视频质量(layer)的需求。将整个过程分成几轮的中国餐馆博弈来进行，每轮中的动作是一致的：
   1. 各UE将已经拥有的layer，自己仍然欠缺的layer，自己的地理位置报告给BS
   2. BS利用“同步进入，完全信息”的中国餐馆模型，计算出每个UE在该轮中的动作（售卖某layer/购买自己欠缺的layer）
   3. BS将决定告诉各UE
   4. UE根据自己的角色，在该轮售卖、购买。



## 一般的“同步进入、完全信息”的CRG模型

* 规定仅当一个UE拥有了自己所需的全部layer后，才会选择自己拥有的某个layer进行售卖
* 规定可以售卖layer（拥有了全部自己所需layer）的UE的集合为$U$ 
* 用于表示上一次迭代的符号都有上标 ’



1. 初始化U中UE的决定，计算$\{R_1,R_2,\dots,R_L\}$
2. Do:
   1. For each $u_i\in U$:
      1. 根据上一轮迭代中，其他UE（$U/\{u_i\}$）做出的售卖决定$x_j ^{'}$，可以得到$N_{-i}^{'} = \{n_{-i,1}^{'},n_{-i,2}^{'},\dots,n_{-i,L}^{'}\}$，即上一轮迭代中，除$u_i$外选择售卖各layer的人数。
      2. $x_i = arg\space max_{l \leq v_i}\space \lambda(R_l,n_{-i,l}^{'}+1)$  //$u_i$ choose layer $l$ which has the highest utility  to relay
3. While(本次U中UE做出的决策和上次不同)//两次迭代结果相同则达到纳什均衡



* 由于D2D存在传输距离的限制，全局对某layer的需求不能反映实际UE周围的需求



## 考虑D2D传输距离的”同步进入、完全信息“的CRG模型

* 规定仅当一个UE拥有了自己所需的全部layer后，才会选择自己拥有的某个layer进行售卖
* 规定可以售卖layer（拥有了全部自己所需layer）的UE的集合为$U$ 
* 规定$u_i$的D2D通讯范围内可以售卖layer的UE的集合为$U_i$
* 规定在$u_i$的D2D通讯范围内需求layer j的UE数为$R_{i,j}$
* 用于表示上一次迭代的符号都有上标 ’
* <img src="https://s2.ax1x.com/2020/01/16/lv9nds.png" alt="image-20200116132427725" style="zoom:67%;" />



1. 初始化U中UE的决定，计算$\{R_{u_i,1},R_{u_i,2},\dots, R_{u_i,L}\}$
2. Do:
   1. For each $u_i\in U$:
      1. 根据上一轮迭代中，$u_i$D2D范围内其他UE（$U_i/\{u_i\}$）做出的售卖决定$x_j ^{'}$，可以得到$N_{-i}^{'} = \{n_{-i,1}^{'},n_{-i,2}^{'},\dots,n_{-i,L}^{'}\}$，即上一轮迭代中，除$u_i$外，在$u_i$的D2D范围内选择售卖各layer的人数。
      2. $x_i = arg\space max_{l \leq v_i}\space \lambda(R_{u_i,l},n_{-i,l}^{'}+1)$  //$u_i$ 选择在自己D2D范围内utility最高的 layer $l$ 进行中继
3. While(本次U中UE做出的决策和上次不同)//两次迭代结果相同则达到纳什均衡



## 2/9 问题整理

* 递归问题：证明递归收敛 or 修改迭代结束判定：两次迭代结果相同或超过迭代次数
* 使用何种评价方法：PSNR or System Utility or 实际收到层数/与其收到层数
* 时间片 =  $T_{BS广播} + k \times(T_{中国餐馆} + T_{D2D})$
* $T_{D2D} = T_{买方提出购买请求} + T_{卖方回复购买请求} + T_{买方选择售卖方} + T_{传输一层layer}$


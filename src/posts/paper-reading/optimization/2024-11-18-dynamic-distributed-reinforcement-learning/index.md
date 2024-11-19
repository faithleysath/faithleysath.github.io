# 基于动作网络的分布式MARL

## 总览

最近在复现“Distributed Multiagent Reinforcement Learning With Action Networks for Dynamic Economic Dispatch”这篇文章。有些细节没太理解，故写点东西来理理顺。

## 问题定义

这篇文章要研究的问题形式如下

$$
\min\sum_{t=1}^T\sum_{i=1}^N\gamma^t F_i(p_{i,t})\\
\text{s.t.} \sum_{i=1}^Np_{i,t}=D_t\\
|p_{i,t}-p_{i,t-1}|<p_i^R
$$

其实就是一个最优化问题，其中决策变量有$T\times N$个。

但是，F_i(p_{i,t})是在t时刻之后是未知的，换句话说，我们必须在t时刻做出t时刻的决策，而不能等到最后再做出所有决策－－我们不能预知未来，也不能时光倒流。

所以针对这种序贯问题，强化学习非常适合用来求解。

## 分布式

这篇文章中对分布式的运用主要在于，每个代理可以领到一个独立的计算任务，而代理之间通过通讯来保持一致性，最终实现全局最优。

在编程上，可以当成在一个中心单元进行计算，只要算法上允许进行计算任务的分解即可。另外，哪怕将分布式算法换成集中式算法，也不影响对问题的求解。

为了方便起见，下面介绍我将先去除分布式的处理。

## 整体架构

这篇文章介绍的算法主要包括两个网络，动作网络和价值网络。

### 价值网络

价值网络有T个，也就是每个时间刻t都有一个独立的价值网络。

之所以这样设计，因为传统的Q函数应该是$Q(s,a)$，即输入是状态和动作，输出是价值Q。但这里的输入不包括$s$。而这个问题中状态定义为$D_t$，在$D_t$的轨迹固定的情况下，$s_t$即固定的，则可以设置T个$Q_t$来消去$s_t$。

价值网络设计为关于$p_{i,t}$的二次函数，其可以被分解成分布式任务。

价值网络的权重更新基于TD差分算法，这被证明是有道理的。

### 动作网络

动作网络有$T\times N$个。这意味着每个代理在每个时间刻$t$都有一个动作网络。动作网络接收输入$D_t$，输出动作。

### 应该使用哪个网络
训练过程中，两个网络同时使用。推理过程中，可以只使用动作网络。

只有动作网络的话，无法评判动作的优劣性，因此无法进行训练。

### 存疑

上述两个网络的训练，是针对同一个$D_t$轨迹，进行$K$个epoch的训练。从参数量上来说，我认为是网络记住了针对这个$D_t$轨迹的最优策略。对其泛化性仍然存疑。这样的话，这种算法只是一个搜索算法。而且需要先经过几个完整的epoch训练后，才能得到最优策略。这就回到我上述的问题，我们不能预知未来，也不能时光倒流。如果条件不允许，面对全新的情况，只允许一次运行的话，这种算法毫无意义。


### 我的理解

对于一个标准的强化学习应用，比如教机器人走路，或者玩超级马里奥。其实也少不了搜索，或者说试错的过程。但是不同的是，代理具有举一反三的能力。

这就有点像插值问题，那些插值点就是试错过程中得到的经验，而中间的通过插值得到的则是类推出来的。

## 网络训练

在前$K_0$个epoch，action网络还没有收敛，所以先用value网络和$\epsilon$贪婪策略去探索和优化。这个过程中，记录下不同方式得到的动作产生的总Cost。并且不断更新其最小值，并将对应方式产生的动作记录到一个序列中和一个记忆池里。

每隔一段轮次，从记忆池里取一个batch出来训练action网络（本质上就是使参数记住这个最优动作）

用这种方法，至少可以保证action网络不会变差。也就是具备单调性。
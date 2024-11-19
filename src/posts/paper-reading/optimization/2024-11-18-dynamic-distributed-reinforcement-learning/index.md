＃ 基于动作网络的分布式MARL

＃＃ 总览

最近在复现“Distributed Multiagent Reinforcement Learning With Action Networks for Dynamic Economic Dispatch”这篇文章。有些细节没太理解，故写点东西来理理顺。

＃＃ 问题定义

这篇文章要研究的问题形式如下

$$
\min\sum_{t=1}^T\sum_{i=1}^N\gamma^t F_i(p_{i,t})
\text{s.t.} \sum_{i=1}^Np_{i,t}=D_t
|p_{i,t}-p_{i,t-1}|<p_i^R
$$

其实就是一个最优化问题，其中决策变量有$T\times N$个。

但是，F_i(p_{i,t})是在t时刻之后是未知的，换句话说，我们必须在t时刻做出t时刻的决策，而不能等到最后再做出所有决策－－我们不能预知未来，也不能时光倒流。

所以针对这种序贯问题，强化学习非常适合用来求解。

＃＃ 分布式

这篇文章中对分布式的运用主要在于，每个代理可以领到一个独立的计算任务，而代理之间通过通讯来保持一致性，最终实现全局最优。

在编程上，可以当成在一个中心单元进行计算，只要算法上允许进行计算任务的分解即可。另外，哪怕将分布式算法换成集中式算法，也不影响对问题的求解。

为了方便起见，下面介绍我将先去除分布式的处理。

＃＃ 整体架构

这篇文章介绍的算法主要包括两个网络，动作网络和价值网络。

＃＃＃ 价值网络

价值网络有T个，也就是每个时间刻t都有一个独立的价值网络。

之所以这样设计，因为传统的Q函数应该是$Q(s,a)$，即输入是状态和动作，输出是价值Q。但这里的输入不包括$s$。而这个问题中状态定义为$D_t$，在$D_t$的轨迹固定的情况下，$s_t$即固定的，则可以设置T个$Q_t$来消去$s_t$。

价值网络设计为关于$p_{i,t}$的二次函数，其可以被分解成分布式任务。

价值网络的权重更新基于TD差分算法，这被证明是有道理的。

＃＃＃动作网络

动作网络有$T\times N$个。这意味着每个代理在每个时间刻$t$都有一个动作网络。动作网络接收输入$D_t$ 
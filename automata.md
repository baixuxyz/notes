# 自动机

自动机形式化描述：

$\mathcal  {A} (Q, \Sigma, \delta, s, F)$ 

- $Q$：状态集。

- $\Sigma$：字母表。

- $\delta$：状态转移函数。

  $\delta : Q \times \Sigma \rightarrow Q$，记为 $\delta(q_0, a) = q_1$，其中，$q_o, q_1 \in Q, a \in \Sigma $。

- $s$：开始状态，$s \in Q$。

- $F$：终止状态集，$F \subseteq Q$。




状态转移函数：

对于给定 $a \in \Sigma$，将状态转移函数 $\delta(q, a)$，记为$\delta_a$，即，$\delta_a : Q \rightarrow Q$。

对于给定 $a, b \in \Sigma$，将函数$\delta_a$ 和 $\delta_b$ 的复合 $\delta_a \circ \delta_b$，记为 $\widehat\delta_{ab}$ 。

连续地应用函数复合，可以得到，$\widehat\delta_{q_0q_1q_2 \cdots} = \delta_{q_0} \circ \delta_{q_1}  \circ \delta_{q_2} \cdots$，注意到，$q_0q_1q_2 \cdots$ 是 $\Sigma^{+}$ 中的元素，则：

$\widehat\delta : Q \times \Sigma^{+} \rightarrow Q$，记为 $ \widehat\delta(q_0, \omega) =   q_1 $，其中 $q_0, q_1 \in Q, \omega \in \Sigma^{+}$。

以上为***确定型自动机***。



自动机接受的语言：

语言 $L =  \{\omega | \omega \in \Sigma^{*}, \widehat\delta(q_0, \omega) \in F \}$。



自动机种类：

通过对自动机加以限定，或扩展，可以得到不同的自动机。

1. 若限定状态集 $Q$ 是有限集，则得到，***有限状态自动机***。
2. 若，同时存在 $ \widehat\delta(q_0, \omega) =   q_1$ 及 $ \widehat\delta(q_0, \omega) =   q_2$，且 $q_0 \neq q_1$，则该自动机为***非确定自动机***。
3. 若在状态转移时允许接受空串，则可以得到，***有 $\epsilon$ 的转移自动机***。形式化地，即允许$\omega = \epsilon$ ，也即， $ \widehat\delta(q_0, \omega) =   q_1 $，其中 $q_0, q_1 \in Q, \omega \in \Sigma^{*}$，

将 1 应用于确定型自动机，则得到，***确定有限自动机***。

将 1，2 组合，则得到，***非确定有限自动机***。

将 3 应用于非确定有限自动机，则得到，***有 $\epsilon$ 转移的非确定有限自动机***。

**确定有限自动机**、**非确定有限自动机**、**有 $\epsilon$ 转移的非确定有限自动机**，这三种自动机的所能接受的语言是相同的，即，这三种自动机的表达（计算）能力是等价的。



自动机的扩展：










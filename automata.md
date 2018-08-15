---
title: 自动机（待续。。。）
date: 2018-08-08
updated: {{ date }}
tags:
  - 自动机

categories: 
  - theory
  - language
---

自动机形式化描述：

$\mathcal  {A} (Q, \Sigma, \delta, s, F)$ 

- $Q$：状态集。

- $\Sigma$：字母表。

- $\delta$：状态转移函数。

  $\delta : Q \times \Sigma \rightarrow Q$，记为 $\delta(q_0, a) = q_1$，其中，$q_o, q_1 \in Q, a \in \Sigma $。

  注（自）：这里的 $\Sigma$ 其实是 $\Sigma^{1}$，即，由一个字符构成的字符串的集合。同理，$\Sigma \cup \{\epsilon\}$ 也即 $\Sigma^{1} \cup \{\epsilon\}$。

- $s$：开始状态，$s \in Q$。

- $F$：终止状态集，$F \subseteq Q$。




状态转移函数：

对于给定 $a \in \Sigma$，将状态转移函数 $\delta(q, a)$，记为$\delta_a$，即，$\delta_a : Q \rightarrow Q$。

对于给定 $a, b \in \Sigma$，将函数$\delta_a$ 和 $\delta_b$ 的复合 $\delta_a \circ \delta_b$，记为 $\widehat\delta_{ab}$ 。

连续地应用函数复合，可以得到，$\widehat\delta_{q_0q_1q_2 \cdots} = \delta_{q_0} \circ \delta_{q_1}  \circ \delta_{q_2} \cdots$，注意到，$q_0q_1q_2 \cdots$ 是 $\Sigma^{+}$ 中的元素，则：

$\widehat\delta : Q \times \Sigma^{+} \rightarrow Q$，记为 $ \widehat\delta(q_0, \omega) =   q_1 $，其中 $q_0, q_1 \in Q, \omega \in \Sigma^{+}$。

以上为***确定型自动机***。



自动机接受的语言：

存在自动机 $M(Q, \Sigma, \delta, s, F)$ ，则，自动机 $M$ 接收的语言 $L(M)$（或 $L$）为：

语言 $L(M) = \{\omega | \omega \in \Sigma^{+}, \widehat\delta(q_0, \omega) \in F \}$，或 $ \{\omega | \omega \in \Sigma^{*}, \widehat\delta(q_0, \omega) \in F \}$。



自动机的等价性：

存在自动机 $M_1$和$M_2$，若，

1. 对于 $\forall \; s \in L(M_1)$， 均有 $s \in L(M_2)$；
2. 对于 $\forall \; s \in L(M_2)$，均有 $s \in L(M_1)$。

也即，$L(M_1) = L(M_2)$，则，称 $M_1$ 与 $M_2$ 等价。

另（自）：自动机的等价性，这一关系是自反、传递、对称的。



自动机种类：

通过对自动机加以限定，或扩展，可以得到不同的自动机。

1. 若限定状态集 $Q$ 是有限集，则得到，***有限状态自动机***。
2. 若同时存在 $ \widehat\delta(q_0, \omega) =   q_1$ 及 $ \widehat\delta(q_0, \omega) =   q_2$，且 $q_0 \neq q_1$，则该自动机为***非确定自动机***。
3. 若 $\delta : Q \times (\Sigma \cup \{ \epsilon \}) \rightarrow Q$，则可以得到，***有 $\epsilon$ 转移的自动机***。

将 1 应用于确定型自动机，则得到，***确定有限自动机***。

将 1，2 组合，则得到，***非确定有限自动机***。

将 3 应用于非确定有限自动机，则得到，***有 $\epsilon$ 转移的非确定有限自动机***。

**确定有限自动机**、**非确定有限自动机**、**有 $\epsilon$ 转移的非确定有限自动机**，这三种自动机的所能接受的语言是相同的，即，这三种自动机的表达（计算）能力是等价的。



自动机的扩展：

***下推自动机***：使用了栈的有限自动机。状态迁移时，需根据当前状态、输入字符和栈顶字符，共同决定迁移之后的状态及栈顶字符。

***线性有界自动机***：

***图灵机***：



## 有限自动机

自动机 $\mathcal  {A} (Q, \Sigma, \delta, s, F)$ ，若 $Q$ 为有限状态集，则自动机 $\mathcal  {A}$ 为**有限自动机**。

**确定有限自动机（$DFA$）**：

$\forall \; q_0, q_1 \in Q, a \in \Sigma$，均有 $q_0 = q_1 \rightarrow  \delta(q_0, a)  = \delta(q_1, a)$。即，状态转移是确定的。

**非确定有限自动机（ $NFA$）**：

$\exists \; q_0, q_1 \in Q, a \in \Sigma$，使得 $q_0 = q_1 \rightarrow  \delta(q_0, a)  \neq   \delta(q_1, a)$。即，状态转移是不确定的。

**有 $\epsilon$ 转移的非确定有限自动机（$NFA-\epsilon$）**：

在非确定有限自动机的基础上，使 $\delta : Q \times (\Sigma \cup \{ \epsilon \}) \rightarrow Q$。即，允许接收空串。



确定有限自动机 $M_{DFA}$、非确定有限自动机 $M_{NFA}$、有 $\epsilon$ 转移的非确定有限自动机 $M_{NFA-\epsilon}$ 之间的等价性（自）：

1. 有限自动机与非确定有限自动机的等价性：

   1. 对于任意 $M_{DFA}$，存在 $M_{NFA}$，使得 $L(M_{DFA}) = L(M_{NFA})$。
   2. 对于任意 $M_{NFA}$，存在 $M_{DFA}$，使得 $L(M_{NFA}) = L(M_{DFA})$。

2. 非确定有限自动机与有 $\epsilon$ 转移的非确定有限自动机的等价性：

   1. 对于任意 $M_{NFA}$，存在 $M_{NFA-\epsilon}$，使得 $L(M_{NFA}) = L(M_{NFA-\epsilon})$。
   2. 对于任意 $M_{NFA-\epsilon}$，存在 $M_{NFA}$，使得 $L(M_{NFA-\epsilon}) = L(M_{NFA})$。

3. 有限自动机与有 $\epsilon$ 转移的非确定有限自动机的等价性：

   1. 对于任意 $M_{DFA}$，存在 $M_{NFA-\epsilon}$，使得 $L(M_{DFA}) = L(M_{NFA-\epsilon})$。
   2. 对于任意 $M_{NFA-\epsilon}$，存在 $M_{DFA}$，使得 $L(M_{NFA-\epsilon}) = L(M_{DFA})$。




（自）$M_{DFA}$、 $M_{NFA}$、$M_{NFA-\epsilon}$ 任意两个自动机之间的等价关系得证，则三个自动机之间的等价关系得证（传递性）。



由定义可知，$DFA$ 是 $NFA$ 的特殊情形，$NFA$ 是 $NFA-\epsilon$ 的特殊情形。

所以，可得 1.1、2.1、3.1。

待续。。。


<!-- 对于 1.2 ，对于任意 $M_{DFA}​$，均可构造 一个  -->










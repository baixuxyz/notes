---
title: 形式文法
updated: 2018-08-10
date: 2018-08-03

tags:
  - 形式文法

categories: 
  - theory
  - language
---



形式文法描述： 

$G(N, \Sigma, P, S)$ 

- $N$ ：非终结符集合。

- $\Sigma$ ：终结符集合。

- 产生式规则集 $P$ :  
  $x \rightarrow y$ ，其中  $x, y \in ( N \cup \Sigma )^{*}$，且，字符串 $x$ 中至少包含一个 $N$ 中的字符。   
  注：这里的 $( N \cup \Sigma )^{*}$ 是指 $N \cup \Sigma $ 的 Kleene 闭包

- $S$ ：起始符号，$S \in N$。 



由形式文法 $G(N, \Sigma, P, S)$ 所产生的***语言***， 是具有如下特点的**字符串集**，这些字符串具有以下特点：

1. 是 $\Sigma^{*}$ 中的元素。即，是 $\Sigma$ 上的字符串。
2. 可以从 $S$ 开始，不断地应用“产生式规则”而构造出。


文法的分类：

不同文法的表达能力（根据文法所生成的语言所确定）是不同的，表达能力是指：不同文法所能描述的语言（即，字符串集）范围。
通过给文法 $G(N, \Sigma, P, S)$ 中的 $P$ 加上一些限定条件，可以的得到描述能力不同的文法。
按照乔姆斯基谱系，可以划分为四个层次：

- 0 型文法  
  语言：递归可枚举语言。

- 1 型文法  
  语言：上下文相关语言。

- 2 型文法  
  语言：上下文无关语言。

- 3 型文法  
  语言：正则语言。



### 3 型文法（正则文法）

文法 $G(N, \Sigma, P, S)$ 

对 $P$ 中的产生式规则加以限定，可以得到两种相似的正则文法。

左线性文法，$P$ 中产生式规则形式如下：

- $A  \rightarrow a$ ，其中，$ A \in N$，且 $a \in \Sigma^{*}$。
- $A  \rightarrow Ba$ ，其中，$A, B \in N$，且 $a  \in \Sigma^{*}$。

右线性文法，$P$ 中产生式规则形式如下：

- $A  \rightarrow a$ ，其中，$ A \in N$，且 $a \in \Sigma^{*}$。
- $A  \rightarrow aB$ ，其中，$A, B \in N$，且 $a  \in \Sigma^{*}$。



### 2 型文法（上下文无关文法）

文法 $G(N, \Sigma, P, S)$ 

对 $P$ 中的产生式规则加以限定，则得到上下文无关文法。

上下文无关文法，$P$ 中产生式规则形式如下：

- $A  \rightarrow \omega$ ，其中，$ A \in N$，且 $\omega \in (N \cup \Sigma)^{*}$。



### 1 型文法（上下文相关文法）

文法 $G(N, \Sigma, P, S)$ 

对 $P$ 中的产生式规则加以限定，则得到上下文相关文法。

上下文相关文法，$P$ 中产生式规则形式如下：

- $\alpha A \beta  \rightarrow \alpha \gamma \beta$ ，其中，$ A \in N$， $\alpha, \beta \in (N \cup \Sigma)^{*}$，且 $\gamma \in (N \cup \Sigma)^{+}$。
- $S \rightarrow \epsilon $，其中 $S$ 是起始符号。



### 0 型文法（无限制文法）

文法 $G(N, \Sigma, P, S)$ 

对 $P$ 中的产生式规则没有任何限定。


逻辑简介与柯里-霍华德同构

## 一个例子

如何利用类型系统实现联合类型？

什么是联合类型？实例 obj，它可能是类型 T1（或其子类）的实例，或者是类型 T2（或其子类）的实例，但不能是其他类型的实例。

Scala 中没有提供原生的联合类型支持（虽然有 Either[T1, T2] 类型，但是使用时需要在经过包装成 Left[T1] 或 Right[T2]），我们可以使用一些“奇技淫巧”来实现原生支持的联合类型。

```scala
import scala.reflect.runtime.universe._
// define union type
type ¬[A] = A => Nothing
type ¬¬[A] = ¬[¬[A]]
type V[A, B] = ¬[¬[A] with ¬[B]]

// test 
typeOf[¬¬[Int]] <:< typeOf[Int V String]
typeOf[¬¬[String]] <:< typeOf[Int V String]

// use
type V_[A, B] = {type λ[X] = ¬¬[X] <:< V[A, B]}
def use[T: (Int V_ String)#λ](t: T) = t match { 
  case i: Int => i
  case str: String => str.length
}

// or
def use_[T: ({type λ[X] = ¬¬[X] <:< V[Int, String]})#λ](t: T) = t match {
  case i: Int => i
  case str: String => str.length
}

// test
> use(1)
> use("abc")
> use(1.1)

> use_(1)
> use_("abc")
> use_(1.1)
```

这一段代码的本质是什么呢？思路从哪儿来？

这一段代码当然可以从类型系统的角度来说明，但它实际上根植于柯里-霍华德同构（curry-howard isomorphism ）。

而柯里-霍华德同构建立起了命题与类型之间的关系。

参考：

http://milessabin.com/blog/2011/06/09/scala-union-types-curry-howard/

## 命题逻辑的语言

古典命题逻辑的语言所用到的字符包含以下三部分：

1. 可数多个命题符号：$A_0, A_1, A_2, …$。
2. 5个联结词：否定符号 $\lnot$ ，合取符号 $\land$， 析取符号 $\lor$， 蕴涵符号 $\rightarrow$ 和双蕴涵符号 $\leftrightarrow$ 。
3. 括号：左括号 “$($”，和右括号“$)$”。

几点说明：

- 在本节中强调的是语法，因此，尽管给联结词起了上述名字，但是在这里应当把它视为完全没有意义的字符。
- $\lnot$ 是一元联结词，其他四个符号是二元联结词。我们用符号 $*$ 来表示四个二元联结词中的任意一个。



规定好基本字符之后，可以定义命题逻辑的语法。我们主要感兴趣的是那些“合乎语法规则”的表达式，称它们为合式公式。

定义如下：

1. 每个命题符号 $A_i$ ，都是合式公式；
2. 如果 $\alpha$ 和 $\beta$ 都是合式公式，则 $(\lnot{\alpha})$，$(\alpha \land \beta)$， $(\alpha \lor \beta)$， $(\alpha \rightarrow \beta)$ 和 $(\alpha \leftrightarrow \beta)$ 也是合式公式；
3. 别无其他。

接下来，定义构造序列如下：

有穷序列 $(\varphi_0, \varphi_1, …, \varphi_n)$ ，为 $\alpha$ 的一个构造序列，若，$\varphi_n = \alpha$ ， 且对每一个 $i \le n$，$\varphi_i$ 是一个命题符号，存在 $j, k \lt i$ 使得 $\varphi_i$ 为 $(\lnot\varphi_i)$ 或$(\varphi_j * \varphi_k)$ 。

我们称 $\alpha$ 为合式公式，若存在 $\alpha$ 的一个构造序列。

需要注意的是，构造序列并不唯一。



既然每一个合式公式都存在一个构造序列。我们可以把自然数上的“数学归纳法”转化为“对公示的归纳法”。有如下形式的归纳原理：

令 $P(\alpha)$ 为一个关于合式公式的性质。假设

1. 对所有的命题符号 $A_i$ ，性质 $P(A_i)$ 成立；并且
2. 对所有的合式公式 $\alpha$ 和 $\beta$ ，如果 $P(\alpha)$ 和 $P(\beta)$ 成立， 则 $P((\lnot\varphi_i))$ 和 $ P((\varphi_j * \varphi_k))$ 成立，

那么 $P(\alpha)$ 对所有的合式公式都成立。

举例：每一个合式公式中左右括号的数目相同。

## 真值指派

首先规定真假值集合为 $\{T, F\}$ ，其中 $T$ 表示真，$F$ 表示假，也可以用 $\{1, 0\}$ 来表示真假值。

令 $S$ 表示命题符号的集合。$S$ 上的真值指派 $v$ 就是从 $S$ 到真假值的一个映射

$$
v: S \rightarrow \{T, F\}
$$
令 $\overline{S}$ 表示只含有 $S$ 中命题符号的公式集，即，$\overline{S}$ 中的公式都是由 $S$ 中的命题符号构造而成。我们把真值指派 $v$ 扩张到 $\overline{S}$ 上得到新函数：

$$
\overline{v}: \overline{S} \rightarrow \{T, F\}
$$
满足：

0. 对于任意 $A \in S$，$\overline{v}(A) = v(A)$；
1. ​
$$
\overline{v}(\lnot{\alpha}))  = 
\begin{equation}
\begin{cases}
 T, & \overline{v}(\alpha) = F, \\
 F, & 其他
\end{cases}
\end{equation}
$$

2. ​
$$
\overline{v}((\alpha \land \beta)) =
   \begin{equation}
   \begin{cases}
    T, & 如果 \, \overline{v}(\alpha) = T \, 且 \,  \overline{v}(\beta) = T  \\
    F, & 其他
   \end{cases}
   \end{equation}
$$

3. ​
$$
\overline{v}((\alpha \lor \beta)) =
      \begin{equation}
      \begin{cases}
       T, & 如果 \, \overline{v}(\alpha) = T \, 或 \,  \overline{v}(\beta) = T  \\
       F, & 其他
      \end{cases}
      \end{equation}
$$

4. ​
$$
\overline{v}((\alpha \rightarrow \beta)) =
      \begin{equation}
      \begin{cases}
       T, & 如果 \, \overline{v}(\alpha) = T \,且 \,  \overline{v}(\beta) = T  \\
       F, & 其他
      \end{cases}
      \end{equation}
$$

5. ​
$$
\overline{v}((\alpha \leftrightarrow \beta)) =
      \begin{equation}
      \begin{cases}
       T, & 如果 \, \overline{v}(\alpha) = \overline{v}(\beta)  \\
       F, & 其他
      \end{cases}
      \end{equation}
$$

我们也可以用真值表来表示 $\overline{v}$ ：
$$
\begin{array}{cccccc}
\hline
\alpha & \beta  & (\lnot{\alpha})  & (\alpha \land \beta) & (\alpha \lor \beta) & (\alpha \rightarrow \beta) & (\alpha \leftrightarrow \beta) \\
\hline
T & T & F & T & T & T & T \\
T & F & F & F & T & F & F \\
F & T & T & F & T & T & F \\
F & F & T & F & F & T & T
\end{array}
$$
只有在真值指派 $v$ 确定以后，公式的真值才有意义。在某种意义上说，逻辑学不关心原子事实的真假，而是怎样处理由逻辑符号生成的复合命题的真假。

我们称一个真值指派 $v$ 满足一个公式 $\varphi$ ，如果 $\overline{v}(\varphi) = T$ 。

定义：我们称一个公式集 $\Sigma$ 重言蕴涵 $\tau$ ，记为 $\Sigma \models \tau$ ，如果每一个满足 $\Sigma$ 中所有公式的真值指派都满足 $\tau$ 。

我们称一个公式 $\tau$ 为一个重言式（记作 $\models \tau$），如果 $\emptyset \models \tau$。这与通常的“重言式在所有真值指派下为真”的说法是一致的。

重言式举例：

1. 结合律：

   $$
   (A \lor (B \lor C)) \leftrightarrow ((A \lor B) \lor C) \\
   (A \land (B \land C)) \leftrightarrow ((A \land B) \land C)
   $$

2. 交换律：
   $$
   (A \lor B) \leftrightarrow (B \lor A) \\
   (A \land B) \leftrightarrow (B \land A)
   $$

3. 分配率：
   $$
   (A \land (B \lor C)) \leftrightarrow ((A \land B) \lor (A \land C)) \\
   (A \lor (B \land C)) \leftrightarrow ((A \lor B) \land (A \lor C))
   $$

4. 双重否定：
   $$
   (\lnot(\lnot A)) \leftrightarrow A
   $$

5. 德摩根定律：
   $$
   (\lnot(A \lor B)) \leftrightarrow ((\lnot A ) \land (\lnot B)) \\
   (\lnot(A \land B)) \leftrightarrow ((\lnot A) \lor (\lnot B))
   $$

6. 其他：

   排中律： $(A \lor (\lnot A))​$；

   矛盾律：$(\lnot(A \land (\lnot A)))$；

   逆否命题：$((A \rightarrow B) \leftrightarrow ((\lnot B) \rightarrow (\lnot A)))$ 。


## 再谈例子

// 简单介绍

开始的时候，看了一个 Scala 的例子，在那里，我们说，它的本质柯里-霍华德同构的。而这个同构是关于逻辑和类型的，那么，他们是如何对应的呢？

上面我们简单的介绍了命题逻辑，接下来我们要接触的逻辑--直觉逻辑，和它有一点少许的不同。

直觉逻辑的语法和命题逻辑是类似的，但是它使用 $\to, \land, \lor, \bot$ 作为基本联结词。把  $\lnot$ 作为 $\lnot A = A \rightarrow \bot$ 的简写处理。同时很多在命题逻辑中的重言式在直觉逻辑中不再是可证明的，如： 排中律，双重否定律等。需要说明的是，在直觉逻辑中双重否定只有 $ A  \rightarrow \lnot(\lnot A) $ 成立。

下面过程中我们需要了解的关于直觉逻辑的知识有：
$$
\lnot A = A \rightarrow \bot \\
A  \rightarrow \lnot(\lnot A) \\
A \lor B = \lnot(\lnot A \land \lnot B )
$$
在 Scala 中，类型的组合方式有：

```scala
A => B
A with B
```

这两个分别对应于直觉逻辑的：
$$
A \rightarrow B \\
A \land B
$$
除此之外，Scala 还有 Bottom Type 即，Nothing。它对应于直觉逻辑中的  $\bot$  。

根据以上关系，我们就可以在 Scala 中构造出联合类型。



将以上关系应用到类型:

逻辑
$$
\lnot A = A \rightarrow \bot \\
A  \rightarrow \lnot(\lnot A) \\
$$
可以映射到：

```scala 
type ¬[A] = A => Nothing  
type ¬¬[A] = ¬[¬[A]]
```

。

逻辑
$$
A \lor B = \lnot(\lnot A \land \lnot B )
$$
可以映射到：

```scala
type V[A, B] = ¬[¬[A] with ¬[B]] // 联合类型
```

***end***。
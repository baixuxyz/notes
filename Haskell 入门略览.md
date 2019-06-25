## Haskell 入门略览

### 函数式听闻

- lambda 表达式
- 柯里化
- 纯函数、副作用、引用透明
- 类型推断
- 模式匹配
- 代数数据类型
- 函数是一等公民
- 惰性求值

等等



### Hello

Hello World

``` haskell
- Helloworld.hs
main = putStrLn "Hello, World!"
```

run:

``` shell
runghc HelloWord.hs
```



### 表达式

数字表达式

`Num`

``` haskell
> 1 + 2 * 3
7
> 3 / 2 - 1
0.5
> 3 `mod` 2
1
```

布尔表达式

`Bool`

``` haskell
> True && False
True
> False || True
True
>
```

字符串表达式

`String`

``` haskell
> '1' : "23"
"123"
> "123" ++ "456"
```

相等性测试

``` haskell
> (1 + 2 - 3) * 2 == 0
True
> True == False
False
> "123" == "123"
True
```

条件表达式

``` haskell
> if True || False then 1 else 2
1
> if True && False then 1 else 2
2
```



以上所有表达式都有严格的类型限定

``` haskell
> 1 + '1'

<interactive>:36:1: error:
    • No instance for (Num Char) arising from a use of ‘+’
    • In the expression: 1 + '1'
      In an equation for ‘it’: it = 1 + '1'
```



其他数据类型

Tuple

``` haskell
> (1,2)
> (1, True)
> (1, True, 'false')
```

List

``` haskell
> [123, 456]
> "123"
> [1,2] ++ [3,4]
```




### 类型

上面的类型

Int、Bool、String、[Char]、Tuple2、Tuple3、[Char]、[String]、[a]



**类型别名**

``` haskell
-- type String = [Char]
> :i String
type String = [Char]

> type StrList = [String]
> :i StrList
type StrList = [String]
```



**函数类型**

``` haskell
-- Share.hs

add :: Int -> Int -> Int
add x y = x + y
```

``` haskell
> :t add
add :: Int -> Int -> Int
```



**类型类**

Eq

``` haskell
> :t (==)
(==) :: Eq a => a -> a -> Bool
```

Ord

``` haskell
> :t (>)
(>) :: Ord a => a -> a -> Bool
```

Enum

``` haskell
-- 皮亚诺公理
> :t (succ)
succ :: Enum a => a -> a
```

Show

可显示类



### 函数

函数定义

``` haskell
-- Share.hs

add :: (Int, Int) -> Int
add (x, y) => x + y

plus :: Int -> Int -> Int
plus x y = x + y
```



特别地，id 函数

``` haskell
> :i id
id :: a -> a 	-- Defined in ‘GHC.Base’
```



**函数的类型**

add

``` haskell
> :t add
add :: (Int, Int) -> Int
```

plus

``` haskell
> :t plus
plus :: Int -> Int -> Int
> let plus_5 = plus 5
plus 5 :: Int -> Int
> :t plus_5
plus_5 :: Int -> Int
```



**柯里化**

``` haskell
> let curry_add = curry add
> :t curry_add
curry add :: Int -> Int -> Int

> :i curry
curry :: ((a, b) -> c) -> a -> b -> c 	-- Defined in ‘Data.Tuple’
```

**逆柯里化**

``` haskell
> let uncurry_add = uncurry add
> :t uncurry_add
uncurry add :: (Int, Int) -> Int

> :i uncurry
uncurry :: (a -> b -> c) -> (a, b) -> c 	-- Defined in ‘Data.Tuple’
```



### $\lambda$ 表达式

**基本形式** 

``` haskell
> let f = (\x -> \y -> x + y)
> :t f
(\x -> \y -> x + y) :: Num a => a -> a -> a

> :t (f 1)
(f 1) :: Num a => a -> a

> f 1 2
3
```

``` haskell
 :t (\x y -> x + y)
(\x y -> x + y) :: Num a => a -> a -> a
```

#### $\lambda$ 演算

**$\alpha$ 替换** 

在不出现命名冲突的前提下，将函数参数重新命名
$$
\lambda x \rightarrow M \equiv_{\alpha}  \lambda y \rightarrow [y/x]M
$$
举例：

``` haskell
\x -> \y -> x + y
-- alpha 等价
\a -> \b -> a + b
```

拓展：自由变量、约束变量



**$\beta$ 化简** 

将参数替换到函数体。（求值）
$$
(\lambda x \rightarrow M) \, N \equiv_{\beta}  [N/x]M )
$$
举例：

``` haskell
(\x -> x + 2) 1
-- beta 等价
1 + 2
```



**$\eta$ 规约** 

简化表达式，消去冗余变量
$$
\lambda x \rightarrow f x \equiv_{eta} f
$$
举例：

``` haskell
> let f_eta = \x -> \y -> x + y


\x -> f x
-- eta 等价
f
```



#### 应用

**列表表达式**

``` haskell
-- 直角三角形
> [(x, y, z) | x <- [1..20], y <- [x..20], z <- [y..20], x^2 + y^2 == z^2]
```



**快速排序**

``` haskell
-- Share.hs

quick_sort :: Ord a => [a] -> [a]
quick_sort [] = []
quick_sort (a : tail) =
  let smaller_sorted = quick_sort [x | x <- tail, x < a]
      bigger_sorted = quick_sort [x | x <- tail, x > a]
  in smaller_sorted ++ [a] ++ bigger_sorted
```

``` haskell
> quick_sort [3, 2,6,1,23,6,2,4,6,2,7,5]
[1,2,3,4,5,6,7,23]
```



#### 拓展(可选)

邱奇数(church number)

0, 对应 $\lambda f.\lambda x. x$ 

succ, 对应 $\lambda n.\lambda f.\lambda x. f (n f x)$ 

1, 对应 $\lambda f.\lambda x. f x$ 

$$
\begin{align*}
(\lambda n.\lambda f.\lambda x. f (n f x))   (\lambda f.\lambda x. x)  
&= \lambda f.\lambda x. f ((\lambda f.\lambda x. x) f x) \\
&= \lambda f.\lambda x. f x


\end{align*}
$$


邱奇数定义

``` haskell
{-# LANGUAGE RankNTypes #-}
-- Share.hs

-- ghci
-- :set -XRankNTypes

type Church = forall t.(t -> t) -> t -> t

-- 邱奇数 0
church_zero :: Church
church_zero = \f -> \x -> x

-- 邱奇, 后继函数
church_succ :: Church -> Church
church_succ ch = \f -> \x -> f (ch f x)
```



邱奇编码

``` haskell
-- Share.hs

church_encoding :: Int -> Church
church_encoding 0 = church_zero
church_encoding n = church_succ (church_encoding (n - 1))
```

邱奇数到自然数

``` haskell
-- Share.hs

church_decoding :: Church -> Int
church_decoding ch = ch (\x -> x + 1) 0
```



举例：

``` haskell
> church_succ church_zero
church_succ church_zero :: (t -> t) -> t -> t
> church_succ (church_succ church_zero)
church_succ (church_succ church_zero) :: (t -> t) -> t -> t


> church_encoding 3
church_encoding 3 :: (t -> t) -> t -> t
> church_encoding 16
church_encoding 16 :: (t -> t) -> t -> t

> church_decoding church_zero
0
> church_decoding (church_succ (church_succ church_zero))
2

> church_decoding (church_encoding 0)
0
> church_decoding (church_encoding 2)
2
```



### 代数数据类型



模式匹配

```haskell
num_to_bool :: Int -> Bool

num_to_bool 0 = False
num_to_bool 1 = True
num_to_bool _ = True

-- 仅仅为了举例
num_and_length :: [Int] -> Int
num_and_length (1 : []) = 1 + 0
num_and_length (2 : []) = 2 + 0
num_and_length _ = error "non match"
```

从上面可以看出，模式匹配是用值或值构造器来匹配的。



#### 类型的定义

**枚举** 

``` haskell
data OwnBool = OwnTrue | OwnFalse deriving (Show)

data Unit = Unit deriving (Show)
```



**参数化类型** 

Option

``` haskell
data Option a = None | Some a deriving (Show)
```

Pair

``` haskell
data Pair a b = Pair a b deriving (Show)
```

OwnEither

``` haskell
data OwnEither a b = OwnLeft a | OwnRight b deriving (Show)
```



**递归类型** 

``` haskell
-- 皮亚诺公理
data Nat = Zero | Succ Nat deriving (Show, Eq, Ord)

data List a = Nil | Cons a (List a) deriving (Show, Eq, Ord)
```



#### 类型的同构

类型 A 与 类型 B 可以相互转换，即，类型 A 的值集与类型 B 的值集一一对应。

``` haskell
-- 仅为说明
fromAToB :: A -> B
fromBToA :: B -> A
```



举例

**Bool 与 OwnBool** 

类型信息

``` haskell
> :i Bool
data Bool = False | True 	-- Defined in ‘GHC.Types’
...
instance Show Bool -- Defined in ‘GHC.Show’
...
> :i OwnBool
data OwnBool = OwnTrue | OwnFalse
instance [safe] Show OwnBool
```

转换函数

``` haskell
bool2own :: Bool -> OwnBool
bool2own False = OwnFalse
bool2own True = OwnTrue

own2bool :: OwnBool -> Bool
own2bool OwnFalse = False
own2bool OwnTrue = True
```



**Maybe 与 Option** 

类型信息

``` haskell
> :i Maybe
data Maybe a = Nothing | Just a 	-- Defined in ‘GHC.Base’
...
instance Show a => Show (Maybe a) -- Defined in ‘GHC.Show’
...
> :i Option
data Option a = None | Some a
instance [safe] Show a => Show (Option a)
```

转换函数

``` haskell
maybe2option :: Show a => Maybe a -> Option a
maybe2option Nothing = None
maybe2option (Just a) = Some a


option2maybe :: Show a => Option a -> Maybe a
option2maybe None = Nothing
option2maybe (Option a) = Just a
```



**列表与自然数** 

List Unit  与 Nat

类型信息

``` haskell
> :i List
data List a = Nil | Cons a (List a)
> :i Nat

data Nat = Zero | Succ Nat
```



转换信息

``` haskell
list2nat :: List Unit -> Nat
list2nat Nil = Zero
list2nat (Cons Unit tail) = Succ (list2nat tail)

nat2list :: Nat -> List Unit
nat2list Zero = Nil
nat2list (Succ nat) = Cons Unit (nat2list nat)
```



*联想* （自）

类型是关于值的集合，类型之间的同构，本质是值集之间的一一对应，即使值集是无穷的，但，只要无穷集合的基数一样，无穷集合之间同样可以一一对应。所以，是否大部分的类型的值，都可以编码到自然数上？



#### ADT

代数数据类型

从一个新的角度来考虑类型，从类型的值集的大小这一角度，来对类型做出分类。



**基本数字(自)** 

0

``` haskell
data Void
```

不存在值构造器，不能创建出 Void 类型的值。



1

``` haskell
> :i Unit
-- 或
> :i ()
```

只能构造出一个该类型的值。



2

Bool 或 Maybe Unit 或 Either Unit

只能构造出两个该类型的值。



$2^{32}$ 

Int

只能构造出 $2^{32}$ 个该字段的值。



...



**和类型** 

``` haskell
> :i Either
data Either a b = Left a | Right b 	-- Defined in ‘Data.Either’
```

该类型的值集的大小，依赖 a、b 类型变量的值集的大小，并且，该类型值集的大小，是 a、b 类型的值集大小之和。

如：Either Bool Unit

它的值集为

``` haskell
> Left True
> Left False
> Right Unit
```

所以 Either Bool Unit 值集的大小为 2 + 1 = 3



**积类型** 

``` haskell
> :i Pair
data Pair a b = Pair a b
instance [safe] (Show a, Show b) => Show (Pair a b)
```

该类型的值集的大小，依赖 a、b 类型变量的值集的大小，并且，该类型值集的大小，是 a、b 类型的值集大小之积。

如：Pair Bool (Either Bool Unit)

它的值集为

``` haskell
> Pair True (Left True)
> Pair True (Left False)
> Pair True (Right Unit)
> Pair False (Left True)
> Pair False (Left False)
> Pair False (Right Unit)
```

所以 Pair Bool (Either Bool Unit) 的值集大小为 2 * 3 = 6



（自）

前面可以看到，模式匹配到的都是值构造器，所以，能够用于模式匹配的类型，都是代数数据类型，且，匹配的都是值构造器。



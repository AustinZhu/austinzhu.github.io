---
title: Church Numeral与Haskell中init函数的联系
author: Austin Zhu
date: 2020-09-07 10:40:00 +0900
categories: [Blogs]
tags: [haskell, lambda calculus]
math: true
comment: true
---

在学习 Haskell 的过程中，碰到了这样的一道习题：使用`fold`函数编写一个`init`函数。通过搜索发现了一个非常优雅的解法：

```haskell
init' [] = error "empty list"
init' xs = foldr f (const []) xs id
    where f x g h = h $ g (x:)
```

仔细一看，这和 Church Numeral 里的`pred`可以说非常相似了!

## Church Numeral

Church Numeral 使用高阶函数来表示自然数。定义如下,

$$
\forall f\space R_f\subseteq D_f\forall x\in D_f:n\space f \space x=f^nx
$$

我们可以在此基础上定义一系列运算：

$$
\begin{align}
\text{succ}&:=\lambda n.\lambda f.\lambda x.f(n\space f \space x) \\
\text{plus}&:=\lambda m.\lambda n.\lambda f.\lambda x.m\space\text{succ}\space n=\lambda m.\lambda n.\lambda f.\lambda x.m\space f\space (n\space f\space x) \\
\text{mult}&:=\lambda m.\lambda n.\lambda f.\lambda x.m\space\text{plus}\space n=\lambda m.\lambda n.\lambda f.\lambda x.m\space (n\space f)\space x \\
\text{exp}&:=\lambda m.\lambda n.\lambda f.\lambda x.m\space\text{mult}\space n=\lambda m.\lambda n.\lambda f.\lambda x.(m\space n)\space f\space x
\end{align}
$$

可以看到以上的运算都是由$$\text{succ}$$（后继函数）衍生出来的，那么如果我们想知道一个数的前趋（predecessor）怎么办呢？

根据定义，应用函数$$n$$次表示$$n$$，如果我们想知道$$n-1$$，只需要少应用一次函数。于是我们可以试想有一个起始函数$$g$$，当对它应用函数$$f$$时，它总是返回一个常量$$x$$。这样当我们对它应用$$n$$次$$f$$时，总是会少应用一次。但是这样起始常量就不是$$x$$了，而是一个函数，所以在之后的 function application 过程中，产生的数也会被 wrap 在函数中（可以理解为把前一次应用$$f$$后的结果放在了一个函数里）。在最后我们需要把包在函数中的 Church Numeral 给取出来。

因此我们需要一些辅助函数:

$$
\begin{align}
\text{inc}&:=\lambda g.\lambda h.h\space(g\space f) \\
\text{id}&:=\lambda u.u \\
\text{extract}&:=k\space\lambda u.u \\
\text{const}&:=\lambda u.x
\end{align}
$$

$$\text{inc}$$用于对包装中的值应用函数，因此$$\text{inc}\space (\text{value}\space v)=\text{value}\space (f\space v)$$且$$\text{inc}\space g=\text{value}\space (g\space f)$$。

$$\text{extract}$$可以将包装中的值取出来，因为$$\text{extract}\space(\text{value}\space v)=v$$而$$\text{value}\space v\space I=v$$，相当于对当前的 wrapper 应用了一次$$\text{id}$$函数。

最后$$\text{const}$$函数用来返回常量，也就是我们的起始函数，它可以实现$$\text{inc const}=\text{value}\space x$$。

代入后$$\text{pred}$$函数就等于

$$
\text{pred}:=\lambda n.\lambda f.\lambda x.\text{extract}\space(n\space\text{inc const})=\lambda n.\lambda f.\lambda x.n\space (\lambda g.\lambda h.h\space(g\space f))\space (\lambda u.x)\space (\lambda u.u)
$$

## 与`init`的联系

我们再看一下`pred`函数：

```haskell
init' xs = foldr f (const []) xs id
    where f x g h = h $ g (x:)
```

此处`(:)`相当于 Church Numeral 定义中的$$f$$，整体思路就是如何应用$$n-1$$次`(:)`，从而构建出一个不包含原列表末尾元素的列表（取列表前$$n-1$$个元素）。我们先来整理一下这里面的各种函数的类型。

```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
const [] :: b -> a
id :: a -> a
f :: a -> (([a] -> [a]) -> [a]) -> ([a] -> [a]) -> [a]
```

简单来说，这里的`h`就是前面的$$\text{value}$$，`g :: ([a] -> [a]) -> [a]`是$$\text{inc}$$，在`fold`过程中会不断被应用。`const []`是起始函数（可以理解为-1），它接受一个任意类型的参数，返回一个空列表。最后`fold`结束后，`id`把我们所需要的列表从`h`中取出来。

看一个例子：

```haskell
init' [1,2,3]
  = foldr f (const []) [1,2,3] id
  = f 1 (foldr f (const []) [2,3]) id
  = f 1 (f 2 (foldr f (const []) [3])) id
  = f 1 (f 2 (f 3 (foldr f (const []) []))) id
  = id $ f 2 (f 3 (foldr f (const []) [])) (1:)
  = id $ (1:) $ f 3 (foldr f (const []) []) (2:)
  = id $ (1:) $ (2:) $ foldr f (const []) [] (3:)
  = id $ (1:) $ (2:) $ const [] (3:)
  = id $ (1:) $ (2:) []
  = id $ (1:) [2]
  = id $ [1,2]
  = [1,2]
```

当然，也有其他的实现方法，[在 ghc 中](https://hackage.haskell.org/package/base-4.14.0.0/docs/src/GHC.List.html#init)，`init`是通过增加一个中间参数来实现的（类比 pair 或 tuple）。这个参数保存了上一次递归产生的 list。相比本文中使用的实现更为直观。（全用最基本的 lambda expression 搭积木真是要命:-(

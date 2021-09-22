---
title: WHNF，HNF和NF的区别
date: 2020-08-04 12:11:00 +0900
author: Austin Zhu
categories: [Notes]
tags: [programming, haskell]
math: true
comment: true
---

最近在查阅 Haskell 的 List 模组时，在`foldl'`的描述中发现了一个专有名词，weak head normal form(WHNF)。于是去阅读了一下相关的定义，为了防止以后忘记，在这里记录一下。

首先，看一下 `foldl'` 的描述：

> `foldl' :: Foldable t => (b -> a -> b) -> b -> t a -> b`
>
> Left-associative fold of a structure but with strict application of the operator.
>
> This ensures that each step of the fold is forced to weak head normal form before being applied, avoiding the collection of thunks that would otherwise occur. This is often what you want to strictly reduce a finite list to a single, monolithic result (e.g. `length`).
>
> For a general `Foldable` structure this should be semantically identical to,
>
> ```
> foldl' f z = foldl' f z . toList
> ```

可以看到，相对于 lazy 的`foldl`，`foldl'` 是严格求值的。这就意味着在 fold 过程中，总会尽可能的做 function application 来减少 thunk，这在很大程度上降低了出现 StackOverflow 的可能。那么 weak head normal form 指的就是 function application 后的一种等价形式。

## WHNF

在 Haskell 中，满足以下其一则为 WHNF：

1. 表达式的最外层为 value construtor
2. 表达式为 lamda 表达式

可以看出，对于一个表达式，WHNF 不是唯一的。

## HNF

HNF 要求 lambda 表达式内部无法进一步的做 $$\beta-reduction$$。

## NF

Normal form 是最为严格的一种 normalization，它要求在表达式的任意位置都无法进一步做$$\beta-reduction$$和$$\eta-reduction$$。给出任意表达式，其 NF 唯一。

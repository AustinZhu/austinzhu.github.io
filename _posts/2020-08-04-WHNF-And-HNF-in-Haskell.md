---
title: Difference Among WHNF，HNF and NF
date: 2020-08-04 12:11:00 +0900
author: Austin Zhu
categories: [Notes]
tags: [programming, haskell]
math: true
comment: true
---

Recently when I was reading the documentation of `foldl'` in the `List` module, I had some difficulty understanding the term *Weak Head Normal Form*.

Here is the description for `foldl'`, a strict version of the lazy `foldl`:

> `foldl' :: Foldable t => (b -> a -> b) -> b -> t a -> b`
>
> Left-associative fold of a structure but with strict application of the operator.
>
> This ensures that each step of the fold is forced to weak head normal form before being applied, avoiding the collection of thunks that would otherwise occur. This is often what you want to strictly reduce a finite list to a single, monolithic result (e.g. `length`).
>
> For a general `Foldable` structure this should be semantically identical to,
>
> ```haskell
> foldl' f z = foldl' f z . toList
> ```

From the description we know that during folding, `foldl'` will apply the function eagerly to reduce thunks. What is a thunk exactly? A thunk is a value that is not fully evaluated. For example, `1+2`.

How is that useful? Well, Haskell is a lazy language, and laziness is handy when working on things like infinite sequences. The evaluation strategy of Haskell is called *call-by-need*. So terms will not be evaluated until it is necessary to do so. Also, once a term is evaluated, its result will be recorded for future usage, which is efficient and desirable (if you live in the world of pure functions). 

However, in some cases you do want strict evaluation, which *might* help you avoid StackOverflow errors. During the evaluation, different normal forms come into play.

## Weak Head Normal Form(WHNF)

An expression is in its weak head normal form if one of the following condition holds：

1. The outmost part(i.e. the root node of the AST ) is a data constructor, or
2. It is a lambda abstraction

WHNF is not unique for an expression.

## Head Normal Form(HNF)

An expression is in its head normal form if no further β-reduction can be done in the lambda body. HNF is automatically an WHNF.

## Normal Form(NF)

Normal form is the most strict normal form. An expression is in its normal form if no further β-reduction and η-reduction can be done. NF is unique for any expression.

---
title: Category Basics
author: Austin Zhu
date: 2021-08-25 16:21:04 +0800
categories: [Blogs]
tags: [category theory, type theory]
math: true
comment: true
---

It seems inevitable that, after one learned Haskell, there's a good possibility she will become a victim of category theory. This is a series(hopefully) of notes about category theory.

Category Theory is ~~the theory using formal method to study the semantics of cats~~ a language that is spoken by mathematicians, just like Design Pattern is a language spoken by senior software engineers. Of course, the latter one reads less exotic and sometimes harmful.

## Definition of Category

![768px-Category_SVG.svg.png (768×768) (wikimedia.org)](https://upload.wikimedia.org/wikipedia/commons/f/ff/Category_SVG.svg)

A *category* $$\mathcal{C}$$ consists of:

- A collection of *objects*, $$\text{Ob}(\mathcal{C})$$;

- A collection of *morphisms*(*arrows*), $$\text{Mor}(\mathcal{C})$$;
  - For each morphism(arrow) $$f$$, a *domain* object $$A$$ and a *codomain* object $$B$$. $$f:A\rightarrow B$$ and $$dom(f)=A, cod(f)=B$$.
  - The collection of morphisms(arrows) between two objects: domain $$A$$ and codomain $$B$$,  is denoted as $$\text{Hom}(A, B)$$.

- A binary operator $\circ$ called *composition*, satisfying:
  - For any pair of arrows $$f:A\rightarrow B$$ and $$g:B\rightarrow C$$, an arrow $$g\circ f:A\rightarrow C$$.
  - (Associativity) For any $$f:A\rightarrow B, g: B\rightarrow C, h:C\rightarrow D$$, we have $$h\circ(g\circ f)=(h\circ g)\circ f$$.
  - (Identity) For each object $$A$$, an identity arrow $$id_A$$, such that for any arrow $$f$$, $$id_{A}\circ f=f=f\circ id_{A}$$.

## Examples of Category

1. $$\text{Set}$$

   - Object: Set, $$A$$.

   - Arrow: Total function, $$f:A\rightarrow B$$.
   - Composition: Function composition.

2. $$\text{Poset}$$

   - Object: Partially-ordered set, $$(P,\leq)$$.
     - $$\leq$$ is a reflexive, transitive, antisymmetric relation on set $P$.
   - Arrow: Total order-preserving function, $$f:P\rightarrow Q$$, such that $$\forall p,p'\in P.p\leq_{P}p' \rightarrow f(p)\leq_{Q} f(p')$$.

   - Composition: Function composition

3. $$\text{Mon}$$

   - Object: Monoid, $$(M,\cdot, e)$$.
     - $$\cdot$$ is an associative binary operator on set $$M$$.
     - $$e\in M$$ is the identity element.
   - Arrow: Monoid homomorphism, $$f:M\rightarrow N$$, such that $$\forall m,m'\in M.m\cdot_{M}m'=f(m)\cdot_{N}f(m')$$ and $$f(e_{M})=e_{N}$$

   - Composition: Homomorphism(Function) composition

4. $$\Omega\text{-Alg}$$

   - Object: $$\Omega$$-Algebra, $$(A, \Omega)$$
     - 



## Commutative Diagrams


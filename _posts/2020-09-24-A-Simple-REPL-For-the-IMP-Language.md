---
title: A Simple REPL For the IMP Language
author: Austin Zhu
date: 2020-09-24 20:00:34 +0900
categories: [Blogs]
tags: [programming, programming language, haskell]
math: true
comment: true
---

IMP is a simple imperative language described in the book *The Formal Semantics of Programming Languages*. The schema of the language is defined as follows:


$$
\begin{align*}
a:=&n\mid X\mid a_0+a_1\mid a_0-a_1\mid a_0
\times a_1 & (
\text{Aexp})\\
b:=&\textbf{true}\mid\textbf{false}\mid a_0=a_1\mid a_0\leq a_1\mid \neg b\mid b_0 \wedge b_1\mid b_0 \vee b_1& (
\text{Bexp})\\
c:=&\textbf{skip}\mid X:= a\mid c_0;c_1\mid \textbf{if }b\textbf{ then } c_0 \textbf{ else } c_1\mid \textbf{while }b\textbf{ do }c& (
\text{Com})
\end{align*}
$$


The REPL is implemented in Haskell. To play with it, execute `stack build --exec IMP-Parser-exe` in your favorite terminal and start coding in IMP. Notice there appears to be some bugs when evaluating on some expressions. Please feel free to submit any issue on Github and I will try to fix it in future commits.

Here is the link to my project: [IMP-Parser](https://github.com/AustinZhu/IMP-Parser)
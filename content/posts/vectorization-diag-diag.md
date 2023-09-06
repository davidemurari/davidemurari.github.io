+++
date= "2023-09-06"
tags= "vectorization"
title= "Vectorization diag operators"
math= true
+++

We consider a matrix $A\in\mathbb{R}^{n\times n}$, and define the two operators
$$
\mathrm{diag}:\mathbb{R}^{n\times n}\rightarrow \mathbb{R}^n,\,\,A\mapsto \sum_{i=1}^n (e_i^TAe_i)e_i,
$$
$$
\mathrm{diag}^2:\mathbb{R}^{n\times n}\rightarrow \mathbb{R}^{n\times n},\,\,A\mapsto \sum_{i=1}^n (e_i^TAe_i)e_ie_i^T = \mathrm{diag}(\mathrm{diag}(A)).
$$
They are both linear operators. We now write explicitly the vectorized version of both of them. This means that we write the matrix $T\in\mathbb{R}^{n^2\times n^2}$ such that $\mathrm{vec}^{-1}(T\mathrm{vec}(A)) = \mathrm{diag}^2(A)$. To do so, we use the properties of the vectorization operator $\mathrm{vec}:\mathbb{R}^{n\times n}\rightarrow\mathbb{R}^{n^2}$, which outputs a vector where all the columns of the input are placed one below the other.


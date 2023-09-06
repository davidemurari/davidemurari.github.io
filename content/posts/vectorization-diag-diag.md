+++
date= "2023-09-06"
tags= "vectorization"
title= "Vectorization diag operators"
math= true
+++

We consider a matrix $A\in\mathbb{R}^{n\times n}$, and define the two operators
$$
\mathrm{diag}:\mathbb{R}^{n\times n}\rightarrow \mathbb{R}^n,\quad A\mapsto \sum_{i=1}^n (e_i^TAe_i)e_i,
$$
$$
\mathrm{diag}^2:\mathbb{R}^{n\times n}\rightarrow \mathbb{R}^{n\times n},\quad A\mapsto \sum_{i=1}^n (e_i^TAe_i)e_ie_i^T = \mathrm{diag}(\mathrm{diag}(A)).
$$
They are both linear operators. We now write explicitly the vectorized version of the second one. This means that we write the matrix $T\in\mathbb{R}^{n^2\times n^2}$ such that $\mathrm{vec}^{-1}(T\mathrm{vec}(A)) = \mathrm{diag}^2(A)$. To do so, we use the properties of the vectorization operator $\mathrm{vec}:\mathbb{R}^{n\times n}\rightarrow\mathbb{R}^{n^2}$, which outputs a vector where all the columns of the input are stacked one below the other.
$$
\mathrm{vec}(\mathrm{diag}^2(A)) = \sum_{i=1}^n \mathrm{vec}([e_i^TAe_i]e_ie_i^T) = \sum_{i=1}^n \mathrm{vec}(e_ie_i^TAe_ie_i^T)
$$
$$
=\left(\sum_{i=1}^n (e_ie_i^T)\otimes (e_ie_i^T)\right)\mathrm{vec}(A) =:T\mathrm{vec}(A).
$$


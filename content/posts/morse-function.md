+++
date= "2023-03-10"
tags= "morseTheory"
title= "Morse functions"
math= true
+++

Source : https://ncatlab.org/nlab/show/Morse+function

Let $\mathcal{M}$ be a smooth manifold, and $f:\mathcal{M}\rightarrow\mathbb{R}$ a smooth function. 

A point $x\in\mathcal{M}$ is critical for $f$ if $df_x\equiv 0$, i.e. for every $v\in T_x\mathcal{M}$, it holds $df_x(v)=0$.

Let $(U,\varphi)$ be a local chart on $\mathcal{M}$, with $x\in U$, and suppose $\varphi(x)=0\in\mathbb{R}^n$. We can then define the Hessian matrix of $x$ as
$$
\left(\frac{\partial^2(f\circ \varphi^{-1})}{\partial x^i\partial x^j}(0)\right)_{ij}.
$$

We say that $x$ is a **regular critical point** if such Hessian matrix is non-singular. We also remark that if this holds for one local chart, by smoothness of $\mathcal{M}$, it holds for any chart.

We say $f$ to be a **Morse function** if all its critical points are regular. In particular, it is Morse if it has no critical points.

A typical **example** of a Morse function on $\mathbb{R}^n$ is $f(x)=\frac{1}{2}\|x\|^2$, for the Euclidean norm $\|\cdot\|$. Indeed here we have
$$
\nabla f(x)=x = 0\implies x=0
$$ 
and 
$$
\nabla^2 f(x) = I_n
$$
that is clearly non-singular.

---

**Source:** https://en.wikipedia.org/wiki/Morse_theory

Given a regular critical point $p\in\mathcal{M}$ of $f$, there is a set of local coordinates $(x_1,...,x_n)$, on an open neighbourhood $U$ of $p$, such that $x(p)=0$ and
$$
f(x)= f(p) - x_1^2 - ... -x_{I}^2 + x_{I+1}^2+...+x_n^2 
$$
where $I$ is the **index of the critical point** $f$.

>Here add a brief note on what is the index of a critical point.

Consequently regular points are isolated
> Add a brief proof of this
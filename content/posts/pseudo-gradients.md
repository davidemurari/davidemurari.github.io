+++
date="2023-03-10"
title="Pseudo-gradient vector fields"
math=true
tags=["dynamical systems", "morse theory", "differential geometry"]
+++

> Add an image with an example of a pseudo-gradient field

## Pseudo-Gradients
**Source** : Morse Theory and Floer Homology
**Authors** : Michèle Audin, Mihai Damian

In this note we define what are **pseudo-gradient fields**, that are related to the critical points of a **[Morse function](/posts/morse-function)**.

We just work on $\mathcal{M}=\mathbb{R}^n$, but the reasoning can be extended to a generic manifold. Let $f:\mathbb{R}^n\rightarrow\mathbb{R}$, then its gradient is a map $\nabla f:\mathbb{R}^n\rightarrow\mathbb{R}^n$ that in general is defined by

$$
\nabla f(x)^Tv = df_x(v),\quad v\in T_x\mathbb{R}^n,\,x\in\mathbb{R}^n.
$$

Then main properties of the vector field $X(x)=-\nabla f(x)$ are the following

1. $X(x)=0$ if and only if $x$ is a critical point of $f$,
  
2. $\mathcal{L}_Xf \leq 0$, and $=0$ only on the critical points of $f$
  

Let now $V\subset \mathbb{R}^n$, and $f:V\rightarrow\mathbb{R}$ a **Morse function**. Then $X\in\mathfrak{X}(V)$ is a pseudo-gradient field or pseudo-gradient adapted to $f$ if

1. $\mathcal{L}_X f\leq 0$ and $=0$ if and only if $x$ is a critical point of $f$,
  
2. In a **Morse chart** in the neighbourhood of a critical point, $X$ coincides with $-\nabla f$.
  

A quite generic family of vector fields satisfying at least the first property is

$$
X(x) = -P(x)\nabla f(x),\quad P(x)^T=P(x)>0
$$

(see Howse, James W., et al. "An application of gradient-like dynamics to neural networks." *Conference Record Southcon*. IEEE, 1994.)

> Here expand on how pseudo-gradients allow to characterise the **stable** and **unstable** manifolds of the equilibria of $-\nabla f(x)$. These facts allow to prove that these vector fields are [structurally stable](/posts/structural-stability) provided the stable and unstable manifolds of the critical points intersect transversely (see [Morse–Smale system - Wikipedia](https://en.wikipedia.org/wiki/Morse%E2%80%93Smale_system#:~:text=Any%20Morse%20function%20f%20on,form%20a%20Morse%E2%80%93Smale%20system.) ).

Add some important information from [Morse-Smale systems - Scholarpedia](http://www.scholarpedia.org/article/Morse-Smale_systems) where it is studied when these systems are structurally stable.

{{< disqus >}}
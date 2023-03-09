---
layout: archive
collection: posts
date: 2022-08-14
permalink: /Quadratic-First-Integrals/
title: "Geometric integrators that preserve quadratic first integrals"
math: true
---

![Cover photo](/images/Preserving-quadratic-first-integrals.png)

## Introduction to the problem

Real-world physical systems can be usually modelled by systems of ordinary differential equations (ODEs) in some Euclidean space $\mathbb{R}^n$ or on some embedded submanifold $\mathcal{M}\subset\mathbb{R}^n$. The solutions of these ODEs can also be geometrically characterized as integral curves of some vector field $X\in\mathfrak{X}(\mathbb{R}^n)$ or $X\in\mathfrak{X}(\mathcal{M})$. Here, by integral curves, we mean curves to which $X$ is pointwise tangent.
These systems usually can not be solved analytically, but such solutions can be approximated using some numerical integrator, for example, the simple explicit-Euler method. More often than not, physical systems also have some characteristic properties. For example, they can conserve some energy function, or be Hamiltonian, or be the gradient of some scalar function. When this is the case, it might be relevant to reproduce such behaviour even in the numerical approximation of its solutions. The area of numerical analysis that focuses on this last aspect is usually called "Geometric numerical integration".
In this post we will focus on the specific case in which a vector field $X\in\mathfrak{X}(\mathbb{R}^n)$ preserves the spheres $S^{n-1}\subset\mathbb{R}^n$. In other words, we consider an $X$ that has $I(x) = |x|^2$ as a conserved quantity. Vector fields satisfying this property are all and only those satisfying $\nabla I(x)^T X(x) = 2x^TX(x)=0$, i.e. they are pointwise orthogonal to $x\in\mathbb{R}^n$.
This article covers some numerical approaches reproducing this behaviour in the discrete solution. One of the reported solutions involves implicit numerical methods, while two others can be based on explicit Runge--Kutta methods. I want to highlight that these 3 strategies are by no means the only ones known. For example, a relevant family of integrators that I do not discuss are projection methods.

## Conditions on classical Runge--Kutta methods

A classical Runge--Kutta method of $s-$stages, is characterized by a matrix $A\in\mathbb{R}^{s\times s}$ and two vectors $b,c\in\mathbb{R}^n$ that together define the Butcher's tableau of the method. The way in which these 3 elements are involved for the solution approximation of $X\in\mathfrak{X}(\mathbb{R}^n)$, is determined by the following discrete update:
$$
x_{k+1}= \varphi_h(x_k) = x_k + h \sum_{i=1}^s b_i X(x_{k,i}), \,\, x_{k,i} = x_k + h\sum_{j=1}^{i-1} a_{ij} X(x_{k,j}).
$$
Since we are considering autonomous dynamical systems, the $c$ vector is not involved here.
This family of numerical integrators can preserve all linear first integrals; however, not all the methods can preserve all quadratic first integrals. For this to happen, there has to be a specific algebraic relation between $A$ and $b$. We first introduce it and then prove it. Let $B$ be the diagonal matrix having $b$ on the diagonal. Then, a Runge--Kutta method of Tableau $(A,b,c)$ preserves all the quadratic first integrals of $X$ if and only if
$$
A^TB+BA-bb^T=0.
$$
This relation can be translated into $a_{ij}b_i + a_{ji}b_j - b_ib_j = 0$ for any $i,j=1,...,s$.
We now prove this fact, but first, we notice that this can not be true if $A$ is lower triangular, i.e. if the Runge--Kutta method is explicit. For simplicity, we restrict to the first integral $I(x)=\|x\|^2$; however, the same reasoning can be extended to $I_S(x)=x^TSx$ for every symmetric and positive semi-definite matrix $S$. Since the problem we consider is autonomous, to achieve what we want is enough to check that $I(x_1)=I(x_0)$:
$$
I(x_1) &= x_1^Tx_1 = \langle x_0 + h\sum_{i=1}^s b_i X(x_{0,i}), x_0 + h\sum_{j=1}^s b_j X(x_{0,j}) \rangle \\
&=I(x_0) + 2h\sum_{i}b_{i}\langle x_0 ,X(x_{0,i})\rangle + h^2\sum_{i,j}b_ib_j\langle X(x_{0,i}),X(x_{0,j}) \rangle
$$
We now introduce, with a simple trick, the terms $a_{ij}$. Indeed, we recall
$$
x_{0,i} = x_0 + h\sum_{j=1}^s a_{ij}X(x_{0,j})\implies x_0 = x_{0,i}-h\sum_{j}a_{ij}X(x_{0,j}).
$$
We hence replace this expression, in place of $x_{0}$ in the previously obtained result:
$$
\begin{split}
I(x_1) &= I(x_0) + 2h\sum_{i}b_{i}\langle x_{0,i}-h\sum_{j}a_{ij}X(x_{0,j}) ,X(x_{0,i}) \rangle +\\ &+h^2\sum_{i,j}b_ib_j\langle X(x_{0,i}),X(x_{0,j}) \rangle = \\
&= I(x_0) - 2h^2 \sum_{i,j}b_ia_{ij}\langle X(x_{0,j}).X(x_{0,i})\rangle+h^2\sum_{i,j}b_ib_j\langle X(x_{0,i}),X(x_{0,j}) \rangle.
\end{split}
$$
Here, one term vanished because $\nabla I(z)^TX(z)=z^TX(z)=0$ for any $z$. We then can conclude noticing that this expression can be rewritten as follows
$$
I(x_1)=I(x_0)-h^2 \sum_{i,j}\left(b_ia_{ij}+b_ja_{ji}-b_ib_j\right)\langle X(x_{0,i}),X(x_{0,j}) \rangle = I(x_0)
$$
by splitting the first term into two equal parts and exchanging (in one of them) the $i$ with the $j$. A simple example of a Runge--Kutta method that satisfies this property is the implicit Mid-point method, a $1-$stage method with Tableau $A=c=1/2$, $b=1$. Thus
$$
A^TB+BA-bb^T = \frac{1}{2}+\frac{1}{2}-1 = 0
$$
as desired.

## Preserve quadratic invariants with modifications of explicit Runge--Kutta methods

All the Runge--Kutta methods that satisfy the condition introduced in the previous section have to be implicit. Sometimes, it is not desirable to use implicit methods; however, one might still be interested in preserving quadratic invariants, as $|x|^2$. In this section, we briefly go through a result introduced in the paper [1], where a more general theory is also developed.
Consider an explicit Runge--Kutta method with $s$ stages 
$$
x_{k+1}= \varphi_h(x_k) = x_k + h \sum_{i=1}^s b_i X(x_{k,i})
$$
with 
$$
x_{k,i} = x_k + h\sum_{j=1}^{i-1} a_{ij} X(x_{k,j})
$$
being the internal stages. It is simple to notice that in general $|\varphi_h(x_k)|^2\neq |x_k|^2$ and hence that the quadratic first integral of $X$ is not discretely preserved. The question is now: how can we slightly modify $\varphi_h$ to preserve $|x|^2$ still getting a consistent approximation of the analytical solution?
To do so, we can introduce a scalar value $\gamma_k$ as
$$
\Psi_h(x_k)=(1-\gamma_k)\varphi_h(x_k)+\gamma_{k}x_k.
$$
By replacing
$$
\gamma_k = \frac{|\varphi_h(x_k)|^2 - |x_k|^2}{|\varphi_h(x_k)-x_k|^2}
$$
in the previous expression, one can verify that $|x|^2$ will be preserved by $\Psi_h$. Moreover, it is possible to prove that if the base method $\varphi_h$ has order $p\geq 2$, the perturbation $\Psi_h$ will have order $p-1$.
It is important to remark that this construction is based on one assumption on $\varphi_h$. Indeed, call $h\Delta_k = \varphi_h(x_k) - x_k$, then we suppose 
$$
\langle x_k, \Delta_k\rangle \cdot \langle \Delta_k,\Delta_k\rangle\neq 0.
$$
This, for example, says that the construction is not well defined for explicit-Euler where $\Delta_k = X(x_k)$ and it hence holds $\langle \Delta_k , x_k\rangle = 0$ since $|x|^2$ is a first integral of $X$.
An example for which this reasoning can be applied is the Euler-Heun method of order 2. Such a method writes
$$
\quad x_{n,2} = x_n + h X(x_n),\quad x_{n+1} = x_n + \frac{h}{2}\left(X(x_n) + X(x_{n,2})\right) = \varphi_h(x_n)
$$
and the modification $\Psi_h$ can be defined as above. I applied such a modified Euler-Heun method to the simple linear ODE $\dot{x}(t) = (B-B^T)x(t) = X(x(t))$, where $B$ is a randomly generated matrix. This vector field preserves the function $|x|^2$ and we see from the plots below that also the numerical solution does it when computed with $\Psi_h$. As a comparison, I report the plot also for the classical Runge--Kutta method of order 4, based on the default implementation in SciPy of Python.

## Lie group methods

In this last section, we are briefly going through a third possible solution to the problem of preserving the first integral $I(x)=\|x\|^2$. The term "Lie group methods" encapsulates a lot of different approaches. If one wants to read more about them, I suggest some references here : [2], [3], [4], [5].

We briefly see the idea and how to transform any Runge-Kutta method into an integrator preserving $I(x)$. We do this by introducing the notion of homogeneous manifold and of transitive Lie group action.

Given a manifold $\mathcal{M}$, we say it to be homogeneous if there can be defined a Lie group action $\varphi: G\times \mathcal{M}\rightarrow\mathcal{M}$ that is transitive. With transitive group action, we mean an action that allows moving any point $m\in \mathcal{M}$ into any other $m'\in\mathcal{M}$ through the action of some group element $g\in G$. More concisely, $\varphi$ is transitive if and only if
$$
\forall m,m'\in\mathcal{M}\,\,\exists g\in G\text{ such that }\varphi(g,m)=m'.
$$
The sphere $S_r^2\subset\mathbb{R}^3$ of radius $r&gt;0$ is a simple example of homogeneous manifold. Indeed, let $G=SO(3)$ be the special orthogonal group, made by orthogonal matrices that preserve the orientation, i.e. rotation matrices. Then, we can define
$$
\varphi: SO(3)\times S_r^2 \rightarrow S_r^2,\,\,(R,x)\mapsto Rx.
$$
This is a group action since $R=I_3$, the identity matrix, it sends $x$ into itself. Moreover, we have $\varphi(R_2,\varphi(R_1,x))=R_2R_1x=\varphi(R_2R_1,x)$. To check its transitivity, there are various approaches that work. For simplicity, we now work with $S_r^2$ embedded in $\mathbb{R}^3$, i.e. $S^2=g^{-1}(0)$ with $g(x)=|x|^2-r$, $x\in\mathbb{R}^3$, use the coordinates of the ambient space and set $r=1$. We then show that $e_1=(1,0,0)\in S^2$ can be sent to any point $v\in S^2$ thanks to some rotation matrix $M_v\in SO(3)$. From this, to prove that for any pair $x,y\in S^2$ there exists $R_{xy}\in SO(3)$ so that $R_{xy}x=y$, we can simply set $R_{xy}=M_yM_x^{-1}$. Indeed, $R_{xy}x=M_yM_x^{-1}x=M_ye_1=y$ as desired.

To build this $M_v$, we can define the vector $v\in S^2\subset\mathbb{R}^3$ in terms of its ambient space coordinates $v=(v_x,v_y,v_z)\in\mathbb{R}^3$. Then, we see that if  $M_ve_1=v$, necessarily the first column of $M_v$ has to be $v$. Then, we just have to complete $M_v$ to an orthogonal matrix with two other orthonormal vectors as second and third columns. The extension to a generic radius $r&gt;0$ is immediate.

The reason why $S_r^2$ matters, in the preservation of $I(x)$, is that when a vector field $X\in\mathbb{R}^3$ has $I$ as a conserved quantity, it is tangent to the spheres of $\mathbb{R}^3$. Indeed, having a conserved quantity $I$ for a dynamics means that such a dynamics preserves the level sets of $I$. The level sets of $I(x)$ are spheres of radius $r$ determined by the initial condition. In other  words, we have that
$$
x_0\in S_r^2 \implies x(t) \in S^2_r\,\forall t\geq 0
$$
if
$$
\dot{x}(t) = X(x(t)),\quad
x(0) = x_0.
$$

A Lie group $G$ can be associated with its Lie algebra $\mathfrak{g}\simeq T_e G$. To map objects in $\mathfrak{g}$ to those in $G$, there are various mappings available, however, we now focus on the exponential map $\exp:\mathfrak{g}\rightarrow G$ that is a local diffeomorphism about the identity element of $G$.  For matrix Lie groups, this is simply the matrix exponential
$$
\exp(A) = \sum_{k=0}^{+\infty}\frac{A^k}{k!}.
$$

The Lie algebra of $SO(3)$, denoted with $\mathfrak{so}(3)$, is the vector space of skew-symmetric matrices. Indeed, we have that for any matrix $A=B-B^T\in\mathfrak{so}(3)$,
$$
\exp(A)\in SO(3).
$$

The main idea of one class of Lie group integrators is to locally transfer the dynamics from the homogeneous manifold $\mathcal{M}$ to the Lie algebra $\mathfrak{g}$ of the Lie group $G$ acting transitively on it. On this linear space $\mathfrak{g}$ we then perform one time-step with some Runge--Kutta method and then bring such updated position back to $\mathcal{M}$. To work with this framework with an arbitrary Runge--Kutta method, we would need a more detailed presentation, however, for explicit-Euler, this can be seen in a quite simple way.

The most difficult task among those presented in the previous paragraph is the one of lifting/transferring the dynamics on the Lie algebra. This is done thanks to the infinitesimal generator of the group action $\varphi$ defined as
$$
\varphi_{*}(\xi)(x) = \frac{d}{d\varepsilon}\Big\vert_{\varepsilon=0}\varphi(\exp(t\xi),x)
$$
where $\xi\in\mathfrak{g}$ and $x\in\mathcal{M}$. Because of the transitivity of the group action, it can be proven that $\forall Y\in\mathfrak{X}(\mathcal{M})$, there is a function $f_Y:\mathcal{M}\rightarrow \mathfrak{g}$ such that
$$
\varphi_{\*}(f_Y(x))(x) = Y(x),\quad \forall x\in\mathcal{M}.
$$

For the sake of completeness, I write the lifted ODE on $\mathfrak{g}$ coming from this function we have just introduced. However, this expression simplifies considerably when we are just interested in one step with the explicit-Euler method.
$$
\dot{\xi}(t) = d\exp_{\xi(t)}^{-1}\circ f \circ \varphi(\exp(\xi(t)),x)\in T_{\xi(t)}\mathfrak{g},\quad
\xi(0) = 0.
$$

The idea of some Lie group methods is hence to perform one-time step of this ODE with some Runge--Kutta method to get an approximation $\xi_1\approx \xi(h)$. Then, we can use it to update the position of $x$ on $\mathcal{M}$ as
$$
x' = \varphi(\exp(\xi_1),x).
$$

Using explicit-Euler as the Runge--Kutta method of choice, we have that
$$
\xi_1 = 0 + h d\exp_{0}^{-1}\circ f \circ \varphi(\exp(0),x) = hf(x).
$$

Thus, we get the so-called Lie-Euler method as
$$
x' = \varphi(\exp(hf(x)),x).
$$
As a test case, we apply it to the free-rigid body equation that writes
$$
\dot{z}(t) = z(t) \times I^{-1}z(t) = X(z(t)),\quad \text{with }I = diag(I_1,I_2,I_3).
$$

It is immediate to notice that $z^Tz=0$ for any $z$, by the properties of the $\mathbb{R}^3$ cross-product $\times$.
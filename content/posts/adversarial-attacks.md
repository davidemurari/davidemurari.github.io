+++
date= "2023-03-12"
tags= ["adversarial attacks","neural networks"]
title= "Adversarial attacks"
math= true
comments= true
+++

> Need to rewrite a bit the text, to make it more clear, and add references.

In this note we consider the problem of classifying objects into $c$ classes to which they belong. Especially for large dimensional problems, neural networks can provide a very great strategy to approach the problem.

One can think to the objects as points in $\mathbb{R}^n$ and to a classifier as a map $\mathcal{N}:\mathbb{R}^n\rightarrow\mathbb{R}^c$ that maps the points into a configuration where the $c$ classes are linearly separated.

Overall the classifier will usually take the form
$$
\mathcal{N} = \sigma\circ L\circ \bar{\mathcal{N}}
$$
where $L:\mathbb{R}^{c'}\rightarrow\mathbb{R}^c$ is a linear map, $\bar{\mathcal{N}}:\mathbb{R}^n\rightarrow\mathbb{R}^{c'}$ is a non-linear parametric function and $\sigma$ is the soft-max function. The predicted class of the point $x\in\mathbb{R}^n$ will then be assigned as $$\ell(x):=\arg\max_{i=1,...,c}\mathcal{N}(x)_i.$$

We remark that in order to get good results, the map $\mathcal{\bar{N}}$ should send the points of $\mathbb{R}^n$ into $c$ clusters in $\mathbb{R}^{c'}$ that are linearly separable.

We can also say that $\mathcal{N}(x) = [p_1(x),...,p_c(x)]$ is a vector representing the probabilities of $x$ belonging to the $i-$th class. The highest probabilty will assign the predicted class to $x$.

The way in which the parametric map $\bar{\mathcal{N}}$ and the linear function $L$ are found, is usually by optimising a cost function like
$$
L = \frac{1}{N} \sum_{i=1}^N (\ell(x_i)-y_i)^2
$$
where $\{(x_i,y_i)\}_{i=1,..,N}$ are given pairs of points with their respective correct label (i.e. training points).

Neural networks are often very effective in solving this task. However, they have been proven to be very sensitive to specifically tailored perturbations of the input points. These perturbations are called "adversarial" since they are built to fool the classifier. 

A class of perturbations is built based on the quantity
$$
\eta = \partial_x (\ell(x_i)-y_i)^2.
$$
This quantity determines the direction in the space $\mathbb{R}^n$ one needs to insist on in order to have the biggest increment in the value $(\ell(x_i)-y_i)^2$. In other words, this is the direction that locally gives the biggest risk of fooling the network, and making it misclassify a point of the form
$$
\tilde{x}_i = x_i + \varepsilon \frac{\eta}{\|\eta\|}
$$
for even small $\varepsilon>0$.

This is just a simple presentation of the problem. There are many known ways to fool the network, but often this gradient is involved in some way.

The vector $\eta$ tells the direction of maximal sensitivity for the network around the training point $x_i$.

At least theoretically or intuitively, it is hence desirable to have 
1. $\ell(x_i)\approx \ell(x_i+\eta)$ for every $\eta\in\mathbb{R}^n$, $\|\eta\|<\varepsilon$,
2. the loss function being reasonably flat around the training points.

These are not easy conditions to verify or impose, and this is a very active area of research.

Let us neglect the function $\sigma$ for a moment, since it does not change the picture too much. We can now analyse the contributions determining $\eta$. By the chain rule we have

$$
\frac{\partial \ell(x)}{\partial x} = \frac{\partial (L\mathcal{\bar{N}}(x))}{\partial x} = L\frac{\partial \mathcal{\bar{N}}(x)}{\partial x}
$$

This tells us that if $\|L\|$ is reasonably small, what is really influential here is the sensitivity $\mathcal{\bar{N}}$ has to input perturbations.

The norm of $L$ relates to how well separated are the clusters in the space $\mathbb{R}^{c'}$. In other words it relates to the so called "margin" of the network $\mathcal{\bar{N}}. The margin can be studied in the design of the loss function. (? to understand if this is true)

Thus one needs to focus more on the other component of the sensitivity vector $\eta$, which is th sensitivity $\mathcal{\bar{N}}$ has with respect to input perturbations.

We study some components of this problem in the note [Sensitivity of Dynamical systems](/posts/sensitivity-dynamical-systems), thinking to the **interpretation of ResNets as dynamical systems' discretizations** (add note here)
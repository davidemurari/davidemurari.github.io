---
layout: archive
collection: posts
date: 2022-08-14
permalink: /Hierarchical-Resnets/
title: "Hierarchical neural networks"
math: true
---

$\sin{x} = \frac{1}{2}$

**Authors:** Yuying Liu, J. Nathan Kutz, Steven L. Brunton
**Year:** 2020
#### Abstract: 
Nonlinear differential equations rarely admit closed-form solutions, thus requiring numerical time-stepping algorithms to approximate solutions. Further, many systems characterized by multiscale physics exhibit dynamics over a vast range of timescales, making numerical integration computationally expensive due to numerical stiffness. In this work, we develop a hierarchy of deep neural network time-steppers to approximate the flow map of the dynamical system over a disparate range of time-scales. The resulting model is purely data-driven and leverages features of the multiscale dynamics, enabling numerical integration and forecasting that is both accurate and highly efficient. Moreover, similar ideas can be used to couple neural network-based models with classical numerical time-steppers. Our multiscale hierarchical time-stepping scheme provides important advantages over current time-stepping algorithms, including (i) circumventing numerical stiffness due to disparate time-scales, (ii) improved accuracy in comparison with leading neural-network architectures, (iii) efficiency in long-time simulation/forecasting due to explicit training of slow time-scale dynamics, and (iv) a flexible framework that is parallelizable and may be integrated with standard numerical time-stepping algorithms. The method is demonstrated on a wide range of nonlinear dynamical systems, including the Van der Pol oscillator, the Lorenz system, the Kuramoto-Sivashinsky equation, and fluid flow pass a cylinder; audio and video signals are also explored. On the sequence generation examples, we benchmark our algorithm against state-of-the-art methods, such as LSTM, reservoir computing, and clockwork RNN. Despite the structural simplicity of our method, it outperforms competing methods on numerical integration.

#### Key points

- Numerical stiffness can be characterized by dynamics over a vast range of timescales.
- Resolving physics on fast time scales limits simulation times and the ability to model slow timescale processes.
- Runge-kutta solvers are based on Taylor-series exapsnsions, and hence are constrained to small time steps -> **local nature of classical methods**
- **IDEA:** Approximate the flow map $\hat{F}$ with a deep residual neural network
- Since they train on separate time-scales, they can train on few timesteps, so no vanishing-exploding gradient problems
- Training data: $S^{(i)}=\left\{\left(\boldsymbol{x}_t^{(i)}, \boldsymbol{x}_{t+\Delta t}^{(i)}, \boldsymbol{x}_{t+2 \Delta t}^{(i)}, \cdots, \boldsymbol{x}_{t+p \Delta t}^{(i)}\right)\right\}_{i=1}^N$
- Neural network definition and training: $$
\begin{gathered}
\hat{\boldsymbol{x}}_{t+\Delta t_j}=\boldsymbol{x}_t+\hat{\boldsymbol{F}}_j\left(\mathbf{x}, \Delta t_j\right) \\
\hat{\boldsymbol{F}}_j\left(\mathbf{x}, \Delta t_j\right)=\sigma_M\left(\mathbf{A}_{M-1}\left(\cdots \sigma_1\left(\mathbf{A}_0\right) \cdots\right)\right)
\end{gathered}
$$ they use $\sigma(x) = \mathrm{ReLU}(x)$.
- The loss function is simply the mean square error $$
M S E=\frac{1}{N p} \sum_{i=1}^N \sum_{k=1}^p\left(\hat{\boldsymbol{x}}_{t+k \Delta t_j}^{(i)}-\boldsymbol{x}_{t+k \Delta t_j}^{(i)}\right)^2
$$ they use a small $p$ since they train on different timescales separately.
- They couple macroscopic and microscopic models, to take advantage of the simplicity and efficiency of the macroscopic models, as well as the accuracy of the microscopic models.
- Small timesteps are responsible for accurate timestepping over short periods, while larger ones are used to reset the predictions over longer periods. Thus, the latter prevent from error accumulations from the short-time models.
- ![[Screenshot 2022-11-14 at 10.22.23.png]]
- Though the multiscale scheme improves the computational efficiency of neural network time-steppers, they still cannot easily match the efficiency of most classic numerical algorithms. *Indeed, evaluating a neural network model (forward propagation) typically requires more computational effort than applying a classic discretization scheme that only involves a few evaluations of the known vector field.* 
- Our approach outperforms neural networks trained at a single scale, providing an accurate and flexible approach for integrating nonlinear dynamical systems.
- Since deep learning is essentially an interpolation method, to leverage more effective training, new sampling strategies may be proposed to address this problem. This is of crucial importance for deep learning, as neural networks are datahungry and the curse of dimensionality makes it impossible to generate largely enough data sets for high dimensional systems.


#### What can we do better/differently still exploiting their ideas?
1. Classical methods are efficient but only if you can rely on explicit methods for small systems. If we work in a setting where this is not verified, we can still hope for faster NNs than numerical methods.
2. Classical methods can not work if the vector field to integrate is either not known or just known partially. In these cases modelling what is missing of the vector field with another neural network can be useful.
3. The idea of learning long time scales can be interesting also for us. However we need to combine this with some simple strategy to verify how accurate we are, in order to get mathematical guarantees.
4. Having a good way to make predictions for a final time and being flexible on the parameters can be interesting also for modelling purposes. In fact, one could be interested in finding the parameters that lead you to a certain area of the phase space at a final time $T$. The strategy we are working with can be of help here.
5. In general what they do is very interesting but lacks mathematical study. Thus, coupling some ideas they have with what we have done, and adding rigorous mathematical results can already be a good project.



> To extend the ideas they present to parametric ODEs, and equip the procedure with some more mathematical guarantees (similar to the residual based control we were thinking about for short time scales).

Integrating backward as a way to check the accuracy of the longer time scale integrator?
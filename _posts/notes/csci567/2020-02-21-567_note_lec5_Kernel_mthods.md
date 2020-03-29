---
layout: post
title: "[Notes] CSCI 567 lec5 Kernel Methods"
tags: [csci567, eng, note]
excerpt_separator: ---
---

*Credit to: Prof. Yan Liu, CSCI567, Spring 2020*

**outline**
- Motivation
- Dual formulation of linear regression
- Kernel Trick

---

## motivation
(kernel trick provide computational convenience)

Recall the question: how to choose nonlinear basis $\phi: \mathbb{R}^D \rightarrow \mathbb{R}^M$ ?

$$
\boldsymbol{w}^T\phi(\boldsymbol{x})
$$


- neural network is one approach: learn $\phi$ from data (more data driven)
- kernel method is another one: sidestep the issue of choosing $\phi$ by using **kernel functions**. it is predefined

### Case study: regularized linear regression
Kernel methods work for many problems and we take **regularized linear regression** as an example.

$$
\boldsymbol{w}^* =\arg\max_w F(\boldsymbol{w})
= (\boldsymbol{\Phi^Tw } + \lambda\boldsymbol{I})^{-1}\boldsymbol{\Phi^Ty}
$$

where operate in space $\mathbb{R}^M$ and $M$ could be huge for computation

let the fradient of $F(\boldsymbol{w})= \|\boldsymbol{\Phi w - y}\|^2_2 +\lambda \|\boldsymbol{w}\|^2_2$ (primal form) to be 0:

$$
\boldsymbol{\Phi^T(\Phi w - y)} + \lambda\boldsymbol{w} = 0
$$

let 

$$
\boldsymbol{w}^* = \frac{1}{\lambda} \mathbf{\Phi}^T(\boldsymbol{y}- \mathbf{\Phi}\boldsymbol{w}^*)
= \mathbf{\Phi}^T\boldsymbol{\alpha} = \sum^N_{n=1} \alpha_n \phi(\boldsymbol{x}_n)
$$

Thus the least square solution is **a linear combination of features**!

Note this is true for perceptron and many other problems. 

Assuming we know $\alpha$, the prediction of $w^*$ on a new example $\boldsymbol{x}$ is

$$
y = \boldsymbol{w}^{*T}\phi(\boldsymbol{x}) = \sum^N_{n=1}\alpha_n\phi(\boldsymbol{x}_n)^T\phi(\boldsymbol{x})
$$

Therefore we do not really need to know $\boldsymbol{w}^*$. Only inner products in the
new feature space matter!

Kernel methods are exactly about computing inner products without knowing $\phi$

**find $\alpha$**
plugging in $\boldsymbol{w} = \boldsymbol{\Phi}^T\boldsymbol{\alpha}$ into $F(\boldsymbol{w})$ gives

$$
\begin{aligned}
G(\boldsymbol{\alpha}) &= F(\boldsymbol{\Phi}^T\boldsymbol{\alpha}) \\
&=\|\boldsymbol{\Phi}\boldsymbol{\Phi}^T\boldsymbol{\alpha}-\boldsymbol{y}\|^2_2 + \lambda\|\boldsymbol{\Phi}^T\boldsymbol{\alpha}\|^2_2 \\
&=\|\boldsymbol{K\alpha-y}\|^2_2 +\lambda^T\boldsymbol{\alpha^{T} K\alpha} \qquad (\boldsymbol{K=\Phi\Phi}^T) \\
&=\boldsymbol{\alpha^TK^TK\alpha} - 2\boldsymbol{y^TK\alpha}+\lambda \boldsymbol{\alpha^TK\alpha} + \text{cnt.}\\
&=\boldsymbol{\alpha^T} (\boldsymbol{K}^2+\lambda\boldsymbol{K})\boldsymbol{\alpha} - 2\boldsymbol{y^TK\alpha} + \text{cnt.} \qquad (\boldsymbol{K^T=K})

\end{aligned}
$$

This is sometime called the **dual formulation** (convert primal form into a function of another variable) of linear regression.

$\boldsymbol{K}=\Phi\Phi^T \in \mathbb{R}^{N\times N}$ is called **Gram matrix** or **kernel matrix** where the $(i, j)$ entry is and $N$ is the number of training cases

$$
\phi(\boldsymbol{x}_i)^T\phi(\boldsymbol{x}_j)
$$

Gram matrix vs covariance matrix

<table>
  <thead>
    <tr>
      <th></th>
      <th>dimensions</th>
      <th>entry $(i,j)$ </th>
      <th>property</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>$\Phi\Phi^T$</td>
      <td>$N\times N$ (usually bigger)</td>
      <td>$\boldsymbol{\phi}(\boldsymbol{x}_i)^T\boldsymbol{\phi}(\boldsymbol{x}_j)$</td>
      <td rowspan="2">both are symmetric and positive semidefinite</td>
    </tr>
    <tr>
      <td>$\Phi^T\Phi$</td>
      <td> $M\times M$</td>
      <td>$\sum^N_{n=1} \phi(\boldsymbol{x_n})_i\phi(\boldsymbol{x}_n)_j$</td>
    </tr>
  </tbody>
</table>


<!-- |              | dimensions  | enty$(i,j)$                                                                | property                                     |
| ------------ | ----------- | -------------------------------------------------------------------------- | -------------------------------------------- |
| $\Phi\Phi^T$ | $N\times N$ (usually bigger)| $\boldsymbol{\phi}(\boldsymbol{x}_i)^T\boldsymbol{\phi}(\boldsymbol{x}_j)$ | both are symmetric and positive semidefinite |
| $\Phi^T\Phi$ | $M\times M$ | $\sum^N_{n=1} \phi(\boldsymbol{x_n})_i\phi(\boldsymbol{x}_n)_j$            | -->

#### to find $\alpha$

Minimize the dual formulation:

$$
G(\boldsymbol{\alpha})=\boldsymbol{\alpha^T} (\boldsymbol{K}^2+\lambda\boldsymbol{K})\boldsymbol{\alpha} - 2\boldsymbol{y^TK\alpha} + \text{cnt.} 
$$

settig the derivative (w.r.t. $\alpha$) to the 0 we have:

$$
0 = (\boldsymbol{K}^2+\lambda\boldsymbol{K})\boldsymbol{\alpha}-\boldsymbol{Ky} = \boldsymbol{K}((\boldsymbol{K}+\lambda \boldsymbol{I})\boldsymbol{\alpha}- \boldsymbol{y})\\
\boldsymbol{\alpha} = (\boldsymbol{K}+\lambda\boldsymbol{I})^{-1}\boldsymbol{y}
$$

Then we obtain

$$
\boldsymbol{w^*=\Phi^T\alpha=\Phi^T(K+\lambda I)^{-1}y}
$$

Minimizing$F(w)$ give: $\boldsymbol{w}^*=(\boldsymbol{\Phi^T\Phi } + \lambda\boldsymbol{I})^{-1}\boldsymbol{\Phi^Ty}$

Minimizing $G(\alpha)$ gives: $\boldsymbol{w^*=\Phi^T(K+\lambda I)^{-1}y}$

fig

#### Then what is the difference?
First, computing $(\boldsymbol{\Phi\Phi^T } + \lambda\boldsymbol{I})^{-1}$ can be more efficient than computing $(\boldsymbol{\Phi^T\Phi } + \lambda\boldsymbol{I})^{-1}$ when $N≤M$.

More importantly, computing $\boldsymbol{K}=(\boldsymbol{\Phi\Phi^T } + \lambda\boldsymbol{I})^{-1}$ also only requires computing inner products in the new feature space! (no need to compute full feature vector after mapping)

Now we can conclude that the exact form of $\phi(·)$ is not essential; all we need is computing inner products $\boldsymbol{\phi}(\boldsymbol{x}_i)^T\boldsymbol{\phi}(\boldsymbol{x}_j)$. (no need to defined specific mapping function)

For some $\phi$ it is indeed possible to compute $\boldsymbol{\phi}(\boldsymbol{x}_i)^T\boldsymbol{\phi}(\boldsymbol{x}_j)$ without computing/knowing $\phi$. This is the **kernel trick**.

## Kernel Trick

Definition: a function $k : \mathbb{R}^D × \mathbb{R}^D → R$ is called a (positive semidefinite) kernel function if there exists a function $\phi : \mathbb{R}^D → \mathbb{R}^M$ so that for any $\boldsymbol{x,x'} \in \mathbb{R}^D$

$$
k(\boldsymbol{x,x'}) = \phi(\boldsymbol{x}^T)\phi(\boldsymbol{x'})
$$

### Using kernel functions
Choosing a nonlinear basis φ becomes choosing a kernel function.

As long as computing the kernel function is more efficient, we should apply
the kernel trick.

$$
\boldsymbol{K} = \mathbf{\Phi\Phi}^T = \left (
\begin{matrix}
    k(\boldsymbol{x_1,x_1}) & k(\boldsymbol{x_1,x_2}) & \cdots & k(\boldsymbol{x_1,x_N}) \\
    k(\boldsymbol{x_2,x_1}) & k(\boldsymbol{x_2,x_2}) & \cdots & k(\boldsymbol{x_2,x_N}) \\
    \vdots & \vdots & \ddots & \vdots \\
     k(\boldsymbol{x_N,x_1}) & k(\boldsymbol{x_N,x_2}) & \cdots & k(\boldsymbol{x_N,x_N})
\end{matrix}
\right)
$$

In fact, $k$ is a kernel if and only if $\boldsymbol{K}$ is positive semidefinite for any $N$ and any $x_1, x_2, . . ., x_N$ (Mercer theorem).

The prediction on a new example $\boldsymbol{x}$ is 

$$
\boldsymbol{w}^{*T} \phi(\boldsymbol{x}) = \sum^N_{n=1} \alpha_n\phi(\boldsymbol{x}_n)^T\phi(\boldsymbol{x})
=\sum^N_{n=1}\alpha_nk(\boldsymbol{x_n,x})
$$

## two most commonly used kernel functions in practice:

### Polynomial kernel 

$$
 k(\boldsymbol{x, x'}) = (\boldsymbol{x^Tx'}+c)^d
$$

for $c\le0$ and $d$ is a positive integer

### Gassian kernel or Radial basis function (RBF) kernel

$$
k(\boldsymbol{x, x'}) = e^{-\frac{\|\boldsymbol{x-x'}\|^2_2}{2\sigma^2}}
$$

for some $\sigma>0$
Think about what the corresponding $\phi$ is for each kernel.

### Composing kernels

Creating more kernel functions using the following rules:
If $k1(·, ·)$ and $k2(·, ·)$ are kernels, the followings are kernels too
- linear combination: $αk_1(·, ·) + βk_2(·, ·) \qquad \text{if } α, β ≥ 0$ 
- product: $k_1(·,·)k_2(·,·)$
- exponential: $e^{k(·,·)}$
- ···

Verify using the definition of kernel!

## Kernel trick is applicable to many ML algorithms: 
- nearest neighbor classifier
- perceptron
- logistic regression
- SVM
- ···
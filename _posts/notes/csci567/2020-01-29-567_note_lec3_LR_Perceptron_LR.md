---
layout: post
title: "[Notes] CSCI 567 lec3 Linear Regression, Perceptron, Logistic RegressionLinear Regression, Perceptron, Logistic Regressionk"
tags: [csci567, eng, note]
excerpt_separator: ---
---

*Credit to: Prof. Yan Liu, CSCI567, Spring 2020*

**Outline**
- Linear Classifier and Surrogate losses
- Perceptron 
- Logistic regression

---

Discuss binary classification 


## Linear Classifier and Surrogate losses

### Deriving classification algorithms

**Step 1**. Pick a set of models F.

Try linear models, but how to predict a label using $\boldsymbol{w}^T\boldsymbol{x}$?

Sign of $\boldsymbol{w}^T\boldsymbol{x}$ p redicts the label:

$$
\text{sign}(y) = 
\begin{cases}
    +1, & \text{if } y>0 \\
    -1, & \text{if } y\le0
\end{cases}
$$

(Sometimes use sgn for sign too)


### The Models

the set of (separating) hyperplanes:

$$
\mathcal{F} = \{f(\boldsymbol{x}) = \text{sgn}(\boldsymbol{w}^T\boldsymbol{x}) | \boldsymbol{w}\in \mathbb{R}^D\}
$$ 

If the data is linearly separable:

$$
\exist \boldsymbol{w}, \text{sgn}(\boldsymbol{w}^T\boldsymbol{x}_n)=y_n \\
\text{or } y_n\boldsymbol{w}^T\boldsymbol{x}>0
$$

for all $n\in[N]$

For not linearly separable data, can apply nonlinear mapping $\Phi$

[fig]
[fig]

### 0-1 loss
**Step 2:** Define error/loss $L(y',y)$

Most natural classification: **0-1 loss** $L(y',y) = \mathbb{I}[y'\ne y]$
See loss as a function of $z = y\boldsymbol{w}^T\boldsymbol{x}$

$$
\ell_{0-1}(z) = \mathbb{I}[z\le 0]
$$

[fig]

the loss for hyperplane $\boldsymbol{w}$ on example $(\boldsymbol{x}, y)$ is $\ell_{0-1}(y\boldsymbol{w}^T\boldsymbol{x})$

However it is **not convex** (i.e. hard to optimize). Actually, minimizing 0-1 loss is NP-hard in general.

### Some Surrogate Losses
[fig]
- perceptron loss $\ell_\text{perceptron}(z) = \max\{0,-z\}$ 
  - (used in Perception)
- hinge loss $\ell_\text{hinge}(z) = \max\{0,1-z\}$
  - (used in SVM and many others) (still give small punishment when model is not so sure)
- logistic loss $\ell_\text{logistic}(z) = \log(1+e^{-z})$
  - (used in logistic regression; the base of log doesn't matter)

### ML becomes convex optimization 

**Step 3**: Find ERM:

$$\boldsymbol{w}^* = \arg\max_{\boldsymbol{w}\in\mathbb{R}^D}\sum^N_{n=1}=\ell(y_n\boldsymbol{w}^T\boldsymbol{x}_n)$$

where $\ell(.)$ can be perception/hinge/logistic loss
- no closed-form in general (unlike linear regression(which use l-p distance))
- Can apply general convex optimization methods

Note: minimizing perceptron loss does not really make sense (try w = 0), but the algorithm derived from this perspective does.

## Perceptron
It is online learning, base on if cases make mistake or not. Do update on model. 
### Numerical optimization 
Stochastic Gradient Descent applied to perceptron loss
find minimizer of 

$$
\begin{aligned}
    F(\boldsymbol{w}) & = \sum^N_{n=1}\ell_\text{perceptron}(y_n\boldsymbol{w}^T\boldsymbol{x}_n) \\
     &= \sum^N_{n=1}\max\{0,-y_n\boldsymbol{w}^T\boldsymbol{x}_n\}
\end{aligned}
$$

- **Gradient Descent (GD)**: simple and fundamental
- **Stochastic Gradient Descent (SGD)**: faster, effective for large-scale problems

Gradient is sometimes referred to as **first-order** information of a function. Therefore, these methods are called first-order methods.

#### Gradient Descent (GD)
**Goal**: minimize F(w)

**Algorithm**: move a bit in the *negative gradient direction*

$$
\begin{aligned}
    w^{(t+1)} & ← w^{(t)} − \eta\nabla F (w^{(t)}) \\
    \text{(variable)} & \leftarrow \text{(true value)}
\end{aligned}
$$

where $η > 0$ is called **step size** or **learning rate**
- in theory η should be set in terms of some parameters of F 
- in practice we just try several small values (it can be dynamic)

stop until $F(w^{(t)})$ does not change much

##### Why GD
by first-order Taylor approximation:

$$
F(\boldsymbol{w}) \approx F(\boldsymbol{w}^{(t)}) + \nabla F(\boldsymbol{w}^{(t)})^T(\boldsymbol{w}-\boldsymbol{w}^{(t)})
$$

(substitute $\boldsymbol{w}$ with $\boldsymbol{w}^{(t+1)}$)

Then GD ensures (loss function always smaller):

$$
F(\boldsymbol{w}^{(t+1)}) \approx F(\boldsymbol{w}^{(t)}) - \eta\| \nabla F(\boldsymbol{w}^{(t)})\|^2_2 \le F(\boldsymbol{w}^{(t)})
$$

#### Stochastic Gradient Descent (SGD)

- GD: move a bit in the negative gradient direction 
- SGD: move a bit in a **noisy** negative gradient direction

$$
w^{(t+1)} \leftarrow w^{(t)} − \eta\tilde{\nabla} F (w^{(t)})
$$

where $\tilde{\nabla} F (w^{(t)})$ is a random variable (called stochastic gradient)

$$
\mathbb{E}[\tilde{\nabla} F (w^{(t)})] = \nabla F (w^{(t)})
$$

(unbiasedness)
Key point: it could be much **faster** to obtain a stochastic gradient!

##### Convergence Guarantees
Many for both GD and SGD on convex objectives.
They tell you at most how many iterations you need to achieve

$$F(w^{(t)}) − F(w^∗) ≤ \epsilon$$

Even for **nonconvex objectives**, many recent works show effectiveness of GD/SGD.


### Applying (S)GD to perceptron loss

#### Applying GD
**Objective**

$$
F(\boldsymbol{w})= \sum^N_{n=1}\max\{0,-y_n\boldsymbol{w}^T\boldsymbol{x}_n\}
$$

Gradient (or really sub-gradient) is

$$
\nabla F(\boldsymbol{w})= \sum^N_{n=1}-\mathbb{I}[y_n\boldsymbol{w}^T\boldsymbol{x}_n \le 0]y_n\boldsymbol{x}
$$

(only misclassified examples contribute to the gradient)
GD update

$$
w^{(t+1)} ← w^{(t)} + \eta\sum^N_{n=1}\mathbb{I}[y_n\boldsymbol{w}^T\boldsymbol{x}_n \le 0]y_n\boldsymbol{x}
$$

*Slow, because each update makes one pass of the entire training set!*

#### Applying SGD
**One common trick:** pick only one case $n ∈ [N]$ uniformly at random, let

$$
\tilde{\nabla} F (w^{(t)}) = -N\mathbb{I}[y_n\boldsymbol{w}^T\boldsymbol{x}_n \le 0]y_n\boldsymbol{x}
$$
clearly unbiased.

**SGD update** (with η absorbing the constant N)

$$
w^{(t+1)} ← w^{(t)} + \eta\mathbb{I}[y_n\boldsymbol{w}^T\boldsymbol{x}_n \le 0]y_n\boldsymbol{x}
$$

*Fast: each update touches only one data point!*

Conveniently, objective of most ML tasks is a finite sum (over each training point) and the above trick applies!
Exercise: try SGD to minimize RSS for linear regression.

### The Perceptron Algorithm

Perceptron algorithm is SGD with $η = 1$ applied to perceptron loss:

Repeat:
- Pick a data point $x_n$ uniformly at random 
- If $\text{sgn}(\boldsymbol{w}^T\boldsymbol{x}_n) \ne y_n$

$$
\boldsymbol{w}^{(t+1)} ← \boldsymbol{w}^{(t)} + y_n\boldsymbol{x}_n \\
\boldsymbol{w} = \sum_{i\in \text{err}} y_i\boldsymbol{x}_i
$$

Note:
- w is always a **linear combination** of the training examples 
- why $η = 1$? Does not really matter in terms of training error

#### Why does it make sense?
If the current weight w makes a mistake

$$
y_n\boldsymbol{w}^T\boldsymbol{x}_n <0
$$

then after the update $w′ = w + y_n\boldsymbol{x}_n$ we have

$$
y_n\boldsymbol{w'}^T\boldsymbol{x}_n = y_n\boldsymbol{w}^T\boldsymbol{x}_n +y_n^2\boldsymbol{x}_n^T\boldsymbol{x}_n \le y_n\boldsymbol{w}^T\boldsymbol{x}_n
$$

Thus it is more likely to get it right after the update.

If training set is linearly separable 
- Perceptron converges in a finite number of steps
- training error is 0

## Logistic regression
In one sentence: find the minimizer of

$$
\begin{aligned}
  F(\boldsymbol{w}) &= \sum^N_{n=1}\ell_\text{logistic}(y_n\boldsymbol{w}^T\boldsymbol{x}_n)\\
  &=\sum^N_{n=1}\ln(1+e^{-y_n\boldsymbol{w}^T\boldsymbol{x}_n})

\end{aligned}
$$


### A Probabilistic View
Instead of predicting a discrete label, can we predict the **probability** of each label? i.e. regress the probabilities

One way: **sigmoid function + linear model**

$$
\mathbb{P}(y = +1 | \boldsymbol{x};\boldsymbol{w}) = \sigma(\boldsymbol{w}^T\boldsymbol{x}_n)
$$

where σ is the sigmoid function (continues variable -> probability):

$$
\sigma(z) = \frac{1}{1+e^{-z}}
$$

Properties
- between 0 and 1 (good as probability) 
- $\sigma(z) \le 0.5 \Leftrightarrow z \le 0$, consistent with predicting the label with $\text{sgn}(z)$ 
- larger $z ⇒$ larger $σ(z) ⇒$ higher confidence in label 1
- $σ(z) + σ(−z) = 1$ for all z

The probability of label −1 is naturally

$$
1 − \mathbb{P}(y = +1 | \boldsymbol{x}; \boldsymbol{w}) = 1 − σ(\boldsymbol{w}^T\boldsymbol{x}) = σ(-\boldsymbol{w}^T\boldsymbol{x})
$$

and thus

$$
\mathbb{P}(y | \boldsymbol{x}; \boldsymbol{w}) = σ(y\boldsymbol{w}^T\boldsymbol{x}) = \frac{1}{1+e^{-y\boldsymbol{w}^T\boldsymbol{x}}}
$$

#### Maximum Likelihood Estimation (MLE) 
(parametric) (under the assumption of distributing)

Specifically, what is the probability of seeing label $y_1, · · · , y_n$ given $x_1,··· ,x_n$, as a function of some $\boldsymbol{w}$?

$$
P(\boldsymbol{w}) = \prod^N_{n=1}\mathbb{P}(y_n|\boldsymbol{x}_n; \boldsymbol{w})
$$

find $\boldsymbol{w}^*$ that maximize the probability $P(\boldsymbol{w})$.

It is a discriminative classifier (not Generative), because it drop $P(\boldsymbol{x})$ (don' care how data is generated)
the derive process if follow.

$$
\boldsymbol{x} \sim N(\mu, \sigma^2) = N(\boldsymbol{w})\\
$$

$$
\begin{aligned}
  & P(\{\boldsymbol{x}_i, y_i\}|\boldsymbol{w})\\
  & = \prod^N_{i=1}P(\boldsymbol{x}_i, y_i |\boldsymbol{w}) \\
  & = \prod^N_{i=1}P(\boldsymbol{x}_i) P(\boldsymbol{x}_i| y_i ; \boldsymbol{w})
\end{aligned}
$$

$$
\begin{aligned}
  \boldsymbol{w}^* & = \arg\max_\boldsymbol{w} P(\{\boldsymbol{x}_i, y_i\}|\boldsymbol{w})\\
  & = \arg\max_\boldsymbol{w} \prod^N_{i=1}P(\boldsymbol{x}_i, y_i |\boldsymbol{w}) \\
  & = \arg\max_\boldsymbol{w} \prod^N_{i=1}P(\boldsymbol{x}_i) P(y_i| \boldsymbol{x}_i ; \boldsymbol{w})\\
  & = \arg\max_\boldsymbol{w} \sum^N_{i=1}\log(P(\boldsymbol{x}_i) P(y_i| \boldsymbol{x}_i ; \boldsymbol{w}))\\
  & = \arg\max_\boldsymbol{w} \ell(w)
\end{aligned}
$$

where $\ell(w) = \log(P(\boldsymbol{x}_i) P(\boldsymbol{x}_i| y_i ; \boldsymbol{w}))$ called log likelihood

$$
\begin{aligned}
  \boldsymbol{w}^* & = \arg\max_\boldsymbol{w} P(\boldsymbol{w}) = \arg\max_\boldsymbol{w}\mathbb{P}(y_i| \boldsymbol{x}_i ; \boldsymbol{w})\\
  &= \arg\max_\boldsymbol{w} \sum^N_{i=1}\ln(\mathbb{P}(\boldsymbol{x}_i| y_i ; \boldsymbol{w})) 
  = \arg\min_\boldsymbol{w} \sum^N_{i=1}-\ln(\mathbb{P}(\boldsymbol{x}_i| y_i ; \boldsymbol{w}))\\ 
  & = \arg\min_\boldsymbol{w} \sum^N_{i=1}\ln(1+e^{-y\boldsymbol{w}^T\boldsymbol{x}})
  = \arg\min_\boldsymbol{w} \sum^N_{i=1}\ell_\text{logistic}(y\boldsymbol{w}^T\boldsymbol{x})\\
  & = \arg\max_\boldsymbol{w} F(\boldsymbol{w})
\end{aligned}
$$

i.e. minimizing **logistic loss** is exactly doing **MLE** for the sigmoid model!


### Optimization

#### SGD

Notice ($\sigma(z)$ is sigmoid function):

$$
\frac{\partial \ell_\text{logistic}(z)}{\partial z} = \frac{-e^{-z}}{1+e^{-z}} = -\frac{1}{1+e^{-(-z)}} = -\sigma(-z)
$$

Apply SGD again

$$
\begin{aligned}
  \boldsymbol{w} \leftarrow & \boldsymbol{w} - \eta \tilde{\nabla} F(\boldsymbol{w}) \\
  = & \boldsymbol{w} - \eta \nabla_\boldsymbol{w} \ell_\text{logistic}(y_n \boldsymbol{w}^T \boldsymbol{x}_n) \\
  = & \boldsymbol{w} - \eta \left( \left.\frac{\partial \ell_\text{logistic}(z)}{\partial z}\right|_{z=y_n\boldsymbol{w}^T\boldsymbol{x}_n}\right)y_n\boldsymbol{x}_n \\
  = & \boldsymbol{w} + \eta \sigma(y_n\boldsymbol{w}^T\boldsymbol{x}_n)y_n\boldsymbol{x}_n \\
  = & \boldsymbol{w} + \eta \mathbb{P}(y_n\boldsymbol{w}^T\boldsymbol{x}_n)y_n\boldsymbol{x}_n
\end{aligned}
$$

This is a soft version of Perceptron!
(Connection between logistic regression and perceptron)


#### Newton method

Converge much faster then GD (1~6 to 1000)

Second-order of Taylor approximation 

$$
F(\boldsymbol{w}) \approx F(\boldsymbol{w}^{(t)}) + \nabla F(\boldsymbol{w}^{(t)})^T(\boldsymbol{w}-\boldsymbol{w}^{(t)}) + \frac{1}{2}(\boldsymbol{w}-\boldsymbol{w}^{(t)})^T \pmb{H}_t(\boldsymbol{w}-\boldsymbol{w}^{(t)})
$$

where $\pmb{H}_t = \nabla^2F(\pmb{w}^{(t)}) \in \mathbb{R}^{D\times D}$ is the **Hessian** of F at $\boldsymbol{w}^{(T)}$, i.e.,

$$
\pmb{H}_{t, ij} = \left . \frac{\partial^2 F(\pmb{w})}{\partial w_i \partial w_j} \right|_{\boldsymbol{w} = \boldsymbol{w}^{(t)}}
$$

Deriving Newton method: let $\boldsymbol{w}-\boldsymbol{w}^{(t)} = \boldsymbol{w}'$

$$
\begin{aligned}
F(\boldsymbol{w}) \approx & F(\boldsymbol{w}^{(t)}) + \nabla F(\boldsymbol{w}^{(t)})^T\boldsymbol{w}' + \frac{1}{2}\boldsymbol{w}' \pmb{H}_t{\boldsymbol{w}'}^T  \\
= & \frac{1}{2}(\boldsymbol{w}' + \boldsymbol{H}^{-1}_t \nabla F(\boldsymbol{w}^{(t)}))^T \boldsymbol{H}_t (\boldsymbol{w}' + \boldsymbol{H}^{-1}_t \nabla F(\boldsymbol{w}^{(t)})) + \text{cnt}
\end{aligned}
$$

Then try to find the lowest point, i.e.,

$$
\begin{aligned}
\boldsymbol{w}' + \boldsymbol{H}^{-1}_t \nabla F(\boldsymbol{w}^{(t)}) & = 0 \\
\boldsymbol{w}-\boldsymbol{w}^{(t)} &= -\boldsymbol{H}^{-1}_t \nabla F(\boldsymbol{w}^{(t)}) \\
\boldsymbol{w} & =  \boldsymbol{w}^{(t)} - \boldsymbol{H}^{-1}_t \nabla F(\boldsymbol{w}^{(t)}) \\
\end{aligned}
$$

Thus

$$
\boldsymbol{w}^{(t+1)} \leftarrow \boldsymbol{w}^{(t)} - \boldsymbol{H}^{-1}_t \nabla F(\boldsymbol{w}^{(t)}) 
$$

#### Comparison of GD and Newton
Both are iterative optimization procedures, but Newton method
- has no learning rate $\eta$ (so no tuning needed!) 
- converges **super fast** in terms of # iterations needed
  - e.g. how many iterations needed when applied to a quadratic?
- requires **second-order** information and is *slow* each iteration (there are many ways to improve it though) (need update D*D metrics)

#### Applying Newton to logistic loss

$$
\begin{aligned}
  \nabla_\boldsymbol{w}\ell_\text{logistic}(\boldsymbol{w}) = \frac{\partial \ell_\text{logistic}(w)}{\partial w} = -\sigma(-w) \\
  \frac{\partial\sigma(z)}{\partial z} = \frac{e^{-z}}{(1+e^{-z})^2} = \sigma(z)(1-\sigma(z))
\end{aligned}
$$

$$
\begin{aligned}
  \nabla^2_\boldsymbol{w}\ell_\text{logistic}(y_n\boldsymbol{w}^T\boldsymbol{x}_n) 
  & = 
  \left( -\left.\frac{\partial \sigma(z)}{\partial z}\right|_{z=-y_n\boldsymbol{w}^T\boldsymbol{x}_n}\right)(y_n \boldsymbol{x}_n)(-y_n\boldsymbol{x}_n^T) \\
  & = \sigma(y_n\boldsymbol{w}^T\boldsymbol{x}_n)(1-\sigma(y_n\boldsymbol{w}^T\boldsymbol{x}_n))\boldsymbol{x}_n\boldsymbol{x}_n^T
\end{aligned}
$$

Notice $y_n \in \{-1, 1\}$ thus $y_n^2 = 1$ 

Hessian of logistic loss positive semidefinite

can we apply Newton method to perceptron/hinge loss?
- no it may introduce none convex problem
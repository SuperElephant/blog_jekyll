---
layout: post
title: "Latex test"
tags: []
excerpt_separator: ###
---
#### Measure error for on prediction 
The classification error (0-1 loss) is inappropriate fot the continuous outcomes
- absolute error: <code>&#124;</code> prediction - sale price <code>&#124;</code>
- squared error: (prediction - sale price)^2 **(Most common, and usually well behaver)**

Then the **Goal** is to minimize total squared error **Residual Sum of Squares (RSS)**
$$\text{RSS}(\tilde{\boldsymbol{w}})=\sum_n (f(\boldsymbol{x}_n)-y_n)^2 = \sum_n (\tilde{\boldsymbol{x}}_n^T\tilde{\boldsymbol{w}}-y_n)^2$$

Find $\tilde{\boldsymbol{w}}^*=\argmax_{\tilde{\boldsymbol{w}}\in \mathbb{R}^{D+1}}\text{RSS}(\tilde{\boldsymbol{w}})$, i.e. least(mean) squares solution (or empirical risk minimizer)

*Reduce machine learning to optimization problem*
 
 Find stationary point(multivariate calculus)
 
 $$\nabla\text{RSS}(\tilde{\boldsymbol{w}})=2\sum_n \tilde{\boldsymbol{x}}_n(\tilde{\v{x}}_n^T\tilde{\boldsymbol{w}}-y_n) \propto (\sum_n \tilde{\boldsymbol{x}}_n\tilde{\boldsymbol{x}}_n^T)\tilde{\boldsymbol{w}}-\sum_n \tilde{\boldsymbol{x}}_ny_n \\
 = (\tilde{\boldsymbol{X}}^T \tilde{\boldsymbol{X}})\tilde{\boldsymbol{w}}-\tilde{\boldsymbol{X}}^T\boldsymbol{y} = 0 \\
 \rArr \tilde{\boldsymbol{w}}^* = (\tilde{\boldsymbol{X}}^T \tilde{\boldsymbol{X}})^{-1}\tilde{\boldsymbol{X}}^T\boldsymbol{y}
 $$

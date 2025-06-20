---
slug: nn-optimization
date: 2024-05-24
draft: false
categories:
  - ANN
---

# Neural Network Optimization

Good post by Matthew Stewart's "Neural Network Optimization" from June 27, 2019.  
<https://towardsdatascience.com/neural-network-optimization-7ca72d4db3e0>

<!-- more -->

Optimization

The equation below represents the average gradient of the loss function with respect to the model parameters, computed over all training samples

$$g={\frac{1}{m}}\sum_{i}\nabla_{\theta}L(f(x^{(i)};\theta),y^{(i)})$$

where:

- $g$ is the gradient of the loss function with respect to the model parameters $\theta$. The gradient vector is a vector that points in the direction of the steepest increase in a function.
- $m$ is the total number of training samples in the dataset. It’s the denominator in the expression, indicating that we’re averaging the gradients over all samples.
- $\sum_{i}$ indicates that we’re summing over all training samples. The index $i$ ranges from $1$ to $m$.
- $\nabla_\theta$ is the gradient operator with respect to the model parameters $\theta$. It computes the partial derivatives of the loss function with respect to each parameter.
- $L(f(x^{(i)};\theta),y^{(i)})$ represents the loss function evaluated for the $i$-th training sample. It measures the discrepancy between the predicted output $f(x^{(i)};\theta)$ and the actual target $y^{(i)}$.

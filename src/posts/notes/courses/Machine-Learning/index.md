# 机器学习 Machine Learning

Since this course is conducted entirely in English, this document will be written in English. However, Chinese explanations of some key concepts will be provided for better understanding.

由于本课程为全英文课程，所以本文档将以英文为主。但是，我会尽量用中文来解释一些概念，以便于大家理解。

## Introduction

### Supervised or Unsupervised

- **Supervised Learning**: The model is trained on a labeled dataset, i.e., the dataset contains both input data and the corresponding output data. The model learns to map the input data to the output data.
- **Unsupervised Learning**: The model is trained on an unlabeled dataset, i.e., the dataset contains only input data. The model learns to find patterns in the input data.

### Regression or Classification

- **Regression**: The output data is continuous.
- **Classification**: The output data is discrete.

## Linear Regression

### Simple Linear Regression with One Variable

1. Hypothesis Function(假设函数): $h_{\theta}(x) = \theta_0 + \theta_1x$
2. Cost Function(代价函数): $J(\theta_0, \theta_1) = \frac{1}{2m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)}) - y^{(i)})^2$
3. Gradient Descent(梯度下降): $\theta_j := \theta_j - \alpha\frac{\partial}{\partial\theta_j}J(\theta_0, \theta_1)$

### Multiple Linear Regression with Multiple Variables

1. Hypothesis Function: $h_{\theta}(x) = \theta_0 + \theta_1x_1 + \theta_2x_2 + \cdots + \theta_nx_n$
2. Cost Function: $J(\theta_0, \theta_1, \cdots, \theta_n) = \frac{1}{2m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)}) - y^{(i)})^2$
3. The Matrix Form of Cost Function: $J(\theta) = \frac{1}{2m}(X\theta - y)^T(X\theta - y)$
4. Gradient Descent: $\theta_j := \theta_j - \alpha\frac{\partial}{\partial\theta_j}J(\theta_0, \theta_1, \cdots, \theta_n)$
5. The Matrix Form of Gradient Descent: $\theta := \theta - \alpha\frac{1}{m}X^T(X\theta - y)$

### Normalization

1. Feature Scaling: $x_i = \frac{x_i - \mu_i}{s_i}$
2. Mean Normalization: $x_i = \frac{x_i - \mu_i}{\text{max}(x_i) - \text{min}(x_i)}$

### Polynomial Regression

Using one sentence to summarize: regard $x^2, x^3, \cdots, x^n$ as new features.

### Normal Equation

Let $\partial J(\theta) = 0$, then we can get the normal equation: $\theta = (X^TX)^{-1}X^Ty$, where $X$ is the matrix of size $m\times(n+1)$, $y$ is the vector of size $m\times1$, and $\theta$ is the vector of size $(n+1)\times1$.
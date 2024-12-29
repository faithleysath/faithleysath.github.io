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
3. Advantages: faster convergence, avoid overflow

### Polynomial Regression

Using one sentence to summarize: regard $x^2, x^3, \cdots, x^n$ as new features.

### Normal Equation

Let $\partial J(\theta) = 0$, then we can get the normal equation: $\theta = (X^TX)^{-1}X^Ty$, where $X$ is the matrix of size $m\times(n+1)$, $y$ is the vector of size $m\times1$, and $\theta$ is the vector of size $(n+1)\times1$.

## Logistic Regression

### Hypothesis Function

Just wrap the linear regression hypothesis function with a sigmoid function: $h_{\theta}(x) = \frac{1}{1 + e^{-\theta^Tx}}$

### Decision Boundary

The decision boundary is the line that separates the positive and negative examples.

### Cost Function

1. For one example: $J(\theta) = -y\log(h_{\theta}(x)) - (1 - y)\log(1 - h_{\theta}(x))$
2. For all examples: $J(\theta) = -\frac{1}{m}\sum_{i=1}^{m}[y^{(i)}\log(h_{\theta}(x^{(i)})) + (1 - y^{(i)})\log(1 - h_{\theta}(x^{(i)}))]$

### Gradient Descent

$\theta := \theta - \alpha\frac{\partial}{\partial\theta}J(\theta)$

after expand and simplify, we can get: $\theta_j := \theta_j - \alpha\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)}) - y^{(i)})x_j^{(i)}$

### Multiclass Classification

One-vs-all: train $K$ logistic regression classifiers, each one against the rest. Than pick the class with the highest probability.

## Overfitting

### Underfitting

High bias, low variance.

### Overfitting

Low bias, high variance.

### How to Solve Overfitting

1. Reduce the number of features.
2. Regularization.

### Regularization

1. L1 Regularization: $J'(\theta) = J(\theta) + \lambda\sum_{j=1}^{n}|\theta_j|$
2. L2 Regularization: $J'(\theta) = J(\theta) + \lambda\sum_{j=1}^{n}\theta_j^2$

## Neural Networks

### Forward Propagation

1. Input Layer: $a^{(1)} = x$
2. Hidden Layer: $z^{(2)} = \theta^{(1)}a^{(1)}$, $a^{(2)} = g(z^{(2)})$
3. Output Layer: $z^{(3)} = \theta^{(2)}a^{(2)}$, $a^{(3)} = g(z^{(3)})$

### Cost Function for Classification

$J(\theta) = -\frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K}[y_k^{(i)}\log(h_{\theta}(x^{(i)})) + (1 - y_k^{(i)})\log(1 - h_{\theta}(x^{(i)}))]$, where $K$ is the number of classes, $y_k^{(i)}$ is the $k$-th element of the output vector $y^{(i)}$.
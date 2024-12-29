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
---
title: 'A Gentle Introduction To Activation Functions in Deep Learning'
excerpt: "Understand what Activation functions are and why they are need in Deep learning models."
comments: true
categories:
  - Machine-learning
tags:
  - Deep-learning
  - Artificial intelligence
  - Neural networks
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: https://cdn-images-1.medium.com/max/2000/1*3tLHUJWOjUrL5aZlWo56yQ.jpeg
---

### **Introduction**

When you get started with deep learning you will definitely come across the term ***activation functions*** also known as Neural Transfer Functions.
In this blog, I will explain what activation functions are and why they are used in deep learning models.

**NOTE:** *I assume you have a basic understanding of neural networks.*

The goal of Machine Learning/Deep learning algorithms is to recognize patterns in data. This pattern can be considered as a function from a mathematical point of view. The ability of machine learning algorithms to approximate this underlying function in a given data is what makes such algorithms very powerful.
The recognition of this function or pattern makes it possible for the model to predict the output of new data.

The pattern/function underlying data can be simple such as a linear relation and sometimes complex such as a non-linear relation.

![](https://cdn-images-1.medium.com/max/2000/1*GeyQBtPVgcurFjY1LQdlxw.jpeg)

### **A Simple Artificial Neuron**

Deep learning models usually consist of many neurons stacked in layers.
Let’s consider a single neuron for simplicity.

![](https://cdn-images-1.medium.com/max/2000/1*Jsm0NBsPuVUKQvEjdpUOpg.png)

The operations performed by a neuron basically involve multiplication and summation operations which are linear and produce an output.
After this, an activation function is applied to produce the final out of the neuron.

![](https://cdn-images-1.medium.com/max/2000/0*Z5mWm24o4cDVZ5UI.png)

Without applying the activation function, the above will just be like a linear function that maps inputs to outputs.

![](https://cdn-images-1.medium.com/max/2000/0*Y3b42mQ4XMOglknn.png)

This makes the neuron only approximate linear functions. As a result, the model can’t recognize complex patterns in data.

## Why are activation functions needed?

In order for neural networks to approximate non-linear or complex functions, there has to be a way to add a non-linear property to the computation of results. 
Using activation functions serves the purpose of introducing non-linearity into the model. This makes it possible for the deep learning models to find complex patterns in data.

### Can any non-linear function be used as an activation function?

No, before a function can be considered as a good candidate for Deep learning models it should have the following properties:

 1. **Non-linear**
    This is required to introduce non-linearity in the model.

 2. **Monotonic** 
    A function that is either entirely non-increasing or non-decreasing.

 3. **Differentiable**
    Deep learning algorithms update their weights via an algorithm called [backpropagation](https://en.wikipedia.org/wiki/Backpropagation). This algorithm can work when the activation function used is differentiable. ie it’s derivates can be calculated.

## Types of Activation Functions.

Most useful activation functions are non-linear functions. The following lists common activation functions.

### **1. Tahn or Hyperbolic tangent function**

![](https://cdn-images-1.medium.com/max/2000/0*2Ltoo51YGOjC4Dlo)

This function has an upper bound of 1 and a lower bound of -1, therefore it will produce outputs between the ranges of 1 to -1.

### 2. Sigmoid or logistic function

![](https://cdn-images-1.medium.com/max/2000/0*8qWWTH6z50LP28jd.png)

This function outputs values between (0,1) and it is centered at 0.

### 3. Relu (**Rectified Linear Unit)**

![](https://cdn-images-1.medium.com/max/2000/0*FVQSRNxLvditSAqD.png)

This function produces values between 0 to infinity. ie it only outputs positive values.

### 4. Leaky Relu

![](https://cdn-images-1.medium.com/max/2000/0*jdLsPwZWmhZNPtd1.png)

This is a variation of ***Relu. ***unlike Relu, ***Leaky Relu*** allows more output values.
It outputs values between 0.01 to infinity

### 5. Softmax

![](https://cdn-images-1.medium.com/max/2000/0*i20dHyZAjUFzsL_s)

This function is mostly used for multiclass prediction problems and it outputs class probabilities for a given input.

## **Which Activation Function Should I use?**

Activation functions have strengths and weaknesses which is based on how well they allow the model to learn features for generalization.

The choice of activation function also depends on the problem you're trying to solve.

* **Relu** is commonly used for hidden layers and **sigmoid/softmax** is commonly used for the output layer.  
    **sigmoid** for binary classification problems and **softmax** for multiclass classification problems.

* **Tanh** is avoided most of the time due to **dead neuron** problem.

* **Sigmoid** and **Tanh** functions are sometimes avoided due to the vanishing gradient and dead neuron problems.

* If we encounter a **case of dead neurons** in the networks the **leaky ReLU** function is the best choice.

**Resources**

* [https://www.analyticsvidhya.com/blog/2020/01/fundamentals-deep-learning-activation-functions-when-to-use-them/](https://www.analyticsvidhya.com/blog/2020/01/fundamentals-deep-learning-activation-functions-when-to-use-them/)

* [http://www.datastuff.tech/machine-learning/why-do-neural-networks-need-an-activation-function/](http://www.datastuff.tech/machine-learning/why-do-neural-networks-need-an-activation-function/)

* [https://en.wikipedia.org/wiki/Activation_function](https://en.wikipedia.org/wiki/Activation_function)

* [https://medium.com/@abhigoku10/activation-functions-and-its-types-in-artifical-neural-network-14511f3080a8](https://medium.com/@abhigoku10/activation-functions-and-its-types-in-artifical-neural-network-14511f3080a8)

In this article, you learned what activation functions are and why they are needed in deep learning models and you also saw commonly used activation functions.
I hope this article served the purpose of introducing you to Activation functions.

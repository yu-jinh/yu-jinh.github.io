---
layout: post
title:  "Softmax and Loss Function of Classification"
date:   2023-05-04
categories: jekyll update
toc: true
img-url: 2023-05-04-Softmax-and-Loss-Function-of-Classification\
---


* TOC
{:toc}

## Summary

During the recent reading of "Dive into Deep Learning", It is pretty interesting to see that
some of the loss function I learned before actually can be derived from a likelihood prespective.
This post will share how softmax function perticipate in the derivation of the loss function
for a simple classification problem. 

## Problem to solve

Assume that we have a minibatch of sample points 
{% raw %}
$$\begin{aligned}
X \in \mathbb{R} ^ {n \times d}
\end{aligned}$$
{% endraw %} of n samples with dimensionality d and
we have q categories in the output, which can be represents by the following linear equation:

{% raw %}
$$\begin{aligned}
\textbf{O} &=  \textbf{WX} + b \\
\hat{\textbf{Y}} &= softmax(\textbf{o})
\end{aligned}$$
{% endraw %}

What shold be the loss function for training this model ?

## Softmax

Recall that softmax function is designed to regularize a serise of data to be like a prbability distribution:

{% raw %}
$$\begin{aligned}
\hat{y}_i = softmax(o)_i &=  \frac{exp(o_i)}{\sum_{j=1}^{q} exp(o_j) } \\
\end{aligned}$$
{% endraw %}


## Loss Function
Just like the linear regression, the objective of this training is to maximise the probability of Y given X, meaning that
given a dataset X, the model should be able to generate output Y with high possibility.

{% raw %}
$$\begin{aligned}
p(Y | X) &= \prod_{i=1}^n p(y^{(i)} | x^{(i)})\\
- \log p(Y | X) &= \sum_{i=1}^n - \log p(y^{(i)} | x^{(i)})\\
&= \sum_{i=1}^n l(y^{(i)}, \hat{y}^{(i)})\\
\end{aligned}$$
{% endraw %}

Where

{% raw %}
$$\begin{aligned}
l(y, \hat{y}) = \sum_{j=1}^q y_j \log \hat{y}_j\\
\end{aligned}$$
{% endraw %}

If we use the softmax function to replace  {% raw %}
$$\begin{aligned}
\hat{y}^{(j)}
\end{aligned}$$
{% endraw %}, we will have:

{% raw %}
$$\begin{aligned}
l(y, \hat{y}) &= \sum_{j=1}^q y^{(j)} \log \frac{exp(o_j)}{\sum_{k=1}^{q} exp(o_k) }\\
&= \sum_{j=1}^{q} y_j \log \sum_{k=1}^q exp(o_k) - \sum_{k=1}^q y_jo_j\\
&= \log \sum_{k=1}^q exp(o_k) - \sum_{k=1}^q y_jo_j\\
\end{aligned}$$
{% endraw %}

Then, take the derivative r.w.t {% raw %}
$$\begin{aligned}
o_j
\end{aligned}$$
{% endraw %}, we will see that:

{% raw %}
$$\begin{aligned}
\partial_{o_j} l(y, \hat{y}) = \frac{exp(o_j)}{\sum_{k=1}^{q} exp(o_k) } - y_j = softmax(o_j) - y_j
\end{aligned}$$
{% endraw %}

This is the difference between the predicted distribution and the ground truth distribution.
## Reference

Zhang, A., Lipton, Z.C., Li, M., & Smola, A.J. (2021). Dive into Deep Learning. Release 0.8.0. D2L.ai. http://d2l.ai/
(p. 131-132)
---
layout: post
title:  "Motivation of Using Squred Loss in Linear Regression"
date:   2023-05-01
categories: jekyll update
toc: true
img-url: 2023-05-01-Motivation-of-Squre-Loss-in-Linear-Regression\
---


* TOC
{:toc}

## Summary

This post will briefly discuss the motivation of using squred loss
in Linear Regression in a likelihood prespective. 

## Problem to solve

Supposed we need to train a linear model:

{% raw %}
$$\begin{aligned}
y &=  w^Tx
\end{aligned}$$
{% endraw %}

The loss function usually would be :

{% raw %}
$$\begin{aligned}
L(w) &=  \frac{1}{2} || y - w^T x||^2_{2}
\end{aligned}$$
{% endraw %}

But why we should use the squred loss objective but not something else ?

## Explain in a Likelihood prespective

First, we need to noticed that our observation usually have noise.
And the noise can be generalized to a Gaussian.
Therefore, the linear function could be:

{% raw %}
$$\begin{aligned}
y &=  w^Tx +\epsilon, \epsilon \sim \mathcal{N}(0, \sigma^2)
\end{aligned}$$
{% endraw %}


Because {% raw %}
$$\begin{aligned} \epsilon \sim \mathcal{N}(0, \sigma^2)
\end{aligned}$$
{% endraw %}, we have {% raw %}
$$\begin{aligned}
y &=  w^Tx +\epsilon  \sim \mathcal{N}(w^Tx, \sigma^2)
\end{aligned}$$
{% endraw %}


Recall that the Gaussian destribution is:

{% raw %}
$$\begin{aligned}
p(x) &= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp{(- \frac{1}{2\sigma^2} (x - \mu)^2)}
\end{aligned}$$
{% endraw %}

The likelihood of y given a specific x should be:

{% raw %}
$$\begin{aligned}
p(y | x) &= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp{(- \frac{1}{2\sigma^2} (y - w^Tx)^2)}
\end{aligned}$$
{% endraw %}

Our objective is to maximize the likelihood of y given a specific x:

{% raw %}
$$\begin{aligned}
p(y | X) &= \Pi_{i=1}^{n} p(y^{(i)} | x^{(i)})
\end{aligned}$$
{% endraw %}

Since the cumulative product is not friendly to calculate,
we take logarithm to both side and deal with the negative log-likelihood instead:

{% raw %}
$$\begin{aligned}
-\log{p(y | X)} &= \sum_{i=1}^n \frac{1}{2} \log{(2 \pi \sigma^2)} + \frac{1}{2\sigma^2} (y^{(i)} - w^T x^{(i)})^2
\end{aligned}$$
{% endraw %}


Therefore, to minimize

{% raw %}
$$\begin{aligned}
-\log{p(y | X)} 
\end{aligned}$$
{% endraw %}

we can minimize

{% raw %}
$$\begin{aligned}
\sum_{i=1}^n \frac{1}{2} (y^{(i)} - w^T x^{(i)})^2
\end{aligned}$$
{% endraw %}


Which is exactly the squred loss objective.

## Reference

Zhang, A., Lipton, Z.C., Li, M., & Smola, A.J. (2021). Dive into Deep Learning. Release 0.8.0. D2L.ai. http://d2l.ai/
(p. 89-90)
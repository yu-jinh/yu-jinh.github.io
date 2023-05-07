---
layout: post
title:  "Generalization Gap and Central Limite Theorem"
date:   2023-05-07
categories: jekyll update
toc: true
img-url: 2023-05-07-Generalization-Gap-and-Central-Limite-Theorem\
---


* TOC
{:toc}

## Summary

To solve a machine learning problem, we usually manage to find the pattern of a data distribution.
But since we are not able to observe all the possible datapoints in a distribution, there is a gap between
the pattern we learned empirically and the real distribution. So, what is the gap?

## Problem to solve

Assume that we have a classifier {% raw %}
$$\begin{aligned}f
\end{aligned}$$
{% endraw %} which trained by the empirical dataset {% raw %}
$$\begin{aligned}
\mathcal{D}
\end{aligned}$$
{% endraw %}. The empirical error is :

{% raw %}
$$\begin{aligned}
\epsilon_{\mathcal{D}}(f) &=  \frac{1}{n} \sum_{i=1}^{n} 1 (f(x^{i}) \neq y^{(i)}) \\
\end{aligned}$$
{% endraw %}

If we can boserve all the datapoints of this distribution, which means n approaches infinity, the population error is:

{% raw %}
$$\begin{aligned}
\epsilon(f) &=  \int \int 1 (f(x) \neq y) p(x,y) dx dy\\
\end{aligned}$$
{% endraw %}

How is the gap  between {% raw %}
$$\begin{aligned}
\epsilon_{\mathcal{D}}(f)
\end{aligned}$$
{% endraw %} and {% raw %}
$$\begin{aligned}
\epsilon(f) 
\end{aligned}$$
{% endraw %} behave?

## Central Limite Theorem

Central Limite Theorem guarantees that whenever we possess n random samples
{% raw %}
$$\begin{aligned}
a_1, \dots ,a_n
\end{aligned}$$
{% endraw %} drawn from any distribution with
mean {% raw %}
$$\begin{aligned}
\mu
\end{aligned}$$
{% endraw %} and standard deviation {% raw %}
$$\begin{aligned}
\sigma
\end{aligned}$$
{% endraw %}, as the number of samples n approaches infinity,
the sample average {% raw %}
$$\begin{aligned}
\hat{\mu}
\end{aligned}$$
{% endraw %} approximately tends towards a normal distribution centered at the true mean
and with standard deviation {% raw %}
$$\begin{aligned}
\frac{\sigma}{\sqrt{n}}
\end{aligned}$$
{% endraw %}.


## Generalization gap and Central Limite Theorem

It is straight forward that the population error is the expectation of empirical error: 
{% raw %}
$$\begin{aligned}
\epsilon(f) = \mathbb{E}[\epsilon_{\mathcal{D}}(f)]
\end{aligned}$$
{% endraw %}

The central Limite Theorem tells us that , as the number of examples grows large,
the empirical error should approach the population error at a rate of {% raw %}
$$\begin{aligned}
\mathcal{O}(\frac{1}{\sqrt{n}})
\end{aligned}$$
{% endraw %}


Thus, to estimate our test error twice as precisely, we must collect four times
as large a test set. To reduce our test error by a factor of one hundred, we must collect ten
thousand times as large a test set.

## Example

For a classification problem, since the error can only be either 0 or 1, they are Bernoulli random variables.
Therefore, the mean is {% raw %}
$$\begin{aligned}
\epsilon(f)
\end{aligned}$$
{% endraw %} and the varience is {% raw %}
$$\begin{aligned}
\epsilon(f)(1 - \epsilon(f))
\end{aligned}$$
{% endraw %}

A little investigation of this function reveals that
our variance is highest when the true error rate is close to 0:5 and can be far lower when it is
close to 0 or close to 1. This tells us that the asymptotic standard deviation of our estimate {% raw %}
$$\begin{aligned}
\epsilon_{\mathcal{D}}(f)
\end{aligned}$$
{% endraw %} of the error {% raw %}
$$\begin{aligned}
\epsilon(f)
\end{aligned}$$
{% endraw %} (over the choice of the n test samples) cannot be any greater than{% raw %}
$$\begin{aligned}
\frac{0.5}{\sqrt{n}}
\end{aligned}$$
{% endraw %}.


if we want our test error {% raw %}
$$\begin{aligned}
\epsilon_{\mathcal{D}}(f)
\end{aligned}$$
{% endraw %}
to approximate the population error {% raw %}
$$\begin{aligned}
\epsilon(f)
\end{aligned}$$
{% endraw %} such that one standard deviation corresponds to an
interval of {% raw %}
$$\begin{aligned}
\pm 0.01
\end{aligned}$$
{% endraw %}:

{% raw %}
$$\begin{aligned}
\sqrt{\frac{0.5^2}{n}} &= \pm 0.01\\
n &= 2500
\end{aligned}$$
{% endraw %}

we should collect roughly 2500 samples.

For 2 standard deviation:

{% raw %}
$$\begin{aligned}
\sqrt{\frac{(2 * 0.5)^2}{n}} &= \pm 0.01\\
n &= 10000
\end{aligned}$$
{% endraw %}

we should collect roughly 10000 samples.
## Reference

Zhang, A., Lipton, Z.C., Li, M., & Smola, A.J. (2021). Dive into Deep Learning. Release 0.8.0. D2L.ai. http://d2l.ai/
(p. 151-152)
---
layout: post
title:  "Environment and Distribution Shift"
date:   2023-05-08
categories: jekyll update
toc: true
img-url: 2023-05-08-Environment-and-Distribution-Shift\
---


* TOC
{:toc}

## Summary

Assume that we have trained a classfier, it works well at the time when we trained it. But for some time
in the future, when the featuer space of the input changed, or the distribution of labels changed, or even the
concept of doing this classification changed, what can we do to improve the performance of the old model?

## Type of Distribution Shift
### Covariate Shift
We assume that the distribution of the imput (features) will shift during time, but the conditional distribution
of the output {% raw %}
$$\begin{aligned} p (y | x) 
\end{aligned}$$
{% endraw %} remains the same. atisticians call this covariate
shift because the problem arises due to a shift in the distribution of the covariates (features).
For example, we have a model to classfy cats and dogs image which trained
with the real photos of cats and dogs. Now, we need a classfier to identify cats and dogs cartoon. In this example,
the input has changed, but the output remains the same.
    
### Label Shift
Label shift is the converse problem of Covariate Shift.  We assume that the distribution of the output (labels) will shift during time,
but the conditional distribution
of the input (features) {% raw %}
$$\begin{aligned} p (x | y) 
\end{aligned}$$
{% endraw %} remains the same. For example, we may want to predict diagnoses given their symptoms (or other manifestations), even as
the relative prevalence of diagnoses are changing over time.

### Concept Shift
Concept shift raises when the very definitions of labels can change.iagnostic criteria for mental illness, what passes
for fashionable, and job titles, are all subject to considerable amounts of concept shift.

## Correction of Distribution Shift
### Covariate Shift

Suppose the original feature distribution is p(x) and the new one is q(x). By definition, we know that p(y | x) = q(y | x)
So, the population risk is:

{% raw %}
$$\begin{aligned}
\int \int l(f(x),y)p(x,y)dxdy &= \int \int l(f(x),y)p(y | x) p(x)dxdy\\
&= \int \int l(f(x),y)q(y | x) q(x) \frac{p(x)}{q(x)}dxdy
\end{aligned}$$
{% endraw %}

Therefore, if we can figure out p(x) / q(x), we can use it as a weight to the loss function.

We can use logistic regression to solve this fraction.The logistic regression is trained to classifies p(x) and q(x)
{% raw %}
$$\begin{aligned}
p(z = 1 | x) &= \frac{1}{1 + exp(-h(x))}\\
p(z = -1 | x) &= \frac{exp(-h(x))}{1 + exp(-h(x))}\\
p(z = 1 | x) + p(z = -1 | x) &= 1\\
\end{aligned}$$
{% endraw %}

Also:

{% raw %}
$$\begin{aligned}
p(z = 1 | x) &= \frac{p(x)}{p(x) + q(x)}\\
\frac{p(z = 1 | x)}{p(z = -1 | x)} &= \frac{p(x)}{q(x)}\\
\end{aligned}$$
{% endraw %}

Therefore:
{% raw %}
$$\begin{aligned}
p(z = 1 | x) &= \frac{p(x)}{p(x) + q(x)}\\
\frac{p(z = 1 | x)}{p(z = -1 | x)} &= \frac{p(x)}{q(x)}\\
\end{aligned}$$
{% endraw %}

{% raw %}
$$\begin{aligned}
p(z = 1 | x) &=  \frac{\frac{1}{1 + exp(-h(x))}}{ \frac{exp(-h(x))}{1 + exp(-h(x))}}\\
&= exp(h(x))\\
\end{aligned}$$
{% endraw %}

We now know the weights of the loss.

### Label Shift

Similarly,suppose the original label distribution is p(y) and the new one is q(y). By definition, we know that p(y | x) = q(y | x)
So, the population risk is:

{% raw %}
$$\begin{aligned}
\int \int l(f(x),y)p(x,y)dxdy &= \int \int l(f(x),y)p(x | y) p(y)dxdy\\
&= \int \int l(f(x),y)q(x | y) q(y) \frac{p(y)}{q(y)}dxdy\\
\end{aligned}$$
{% endraw %}

Same here, we should manage to find p(y)/q(y).

First, we need to get the confusion matrix **C** from the original model, which is simply a k * k
matrix, where each column corresponds to the label category (ground truth) and each row
corresponds to our model’s predicted category. Each cell’s value{% raw %}
$$\begin{aligned}
c_{ij}
\end{aligned}$$
{% endraw %} is the fraction of total
predictions on the validation set where the true label was j and our model predicted i.

Then, we can verage all of our models predictions
at test time together, yielding the mean model outputs{% raw %}
$$\begin{aligned}
\mu(\hat{\textbf{y}}) \in \mathbb{R}^k
\end{aligned}$$
{% endraw %} , whose ith element {% raw %}
$$\begin{aligned}
\mu(\hat{y}_i)
\end{aligned}$$
{% endraw %}
is the fraction of total predictions on the test set where our model predicted i.

Under some mild conditions—if our classifier was reasonably accurate in the
first place, and if the target data contains only categories that we have seen before, and if the
label shift assumption holds in the first place (the strongest assumption here), then we can
estimate the test set label distribution by solving a simple linear system.

{% raw %}
$$\begin{aligned}
\textbf{C}p(y) &= \mu(\hat{y}) \\
p(y) &= \textbf{C}^{-1} \mu(\hat{y})
\end{aligned}$$
{% endraw %}

Since q(y) is the original output distribution, which is easy to get from the model, we
can now calculate the p(y)/q(y) for the new weighted loss function.
### Concept Shift

We can use the same approach that we used for training networks to make
them adapt to the change in the data. In other words, we use the existing network weights and
simply perform a few update steps with the new data rather than training from scratch.

## Reference
Please reach out to this material for more explaination.

Zhang, A., Lipton, Z.C., Li, M., & Smola, A.J. (2021). Dive into Deep Learning. Release 0.8.0. D2L.ai. http://d2l.ai/
(p. 158-164)
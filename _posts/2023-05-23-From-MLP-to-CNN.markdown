---
layout: post
title:  "From MLP to CNN"
date:   2023-05-23
categories: jekyll update
toc: true
img-url: 2023-05-23-From-MLP-to-CNN\
---


* TOC
{:toc}

## Introduction

This post will briefly introduce how we develop convolution neural networks from the traditional MLP.
The basic idea is to add some limitations to the fully connected layers in MLP.

## Motivation

For a traditional MLP, it will treat the whole input sample as a single object, which might lose some ability to distill the partial features. For example, if we fed an image to an MLP, the fully connected layer would do linear combinations to each pixel respectively. Therefore, even if we do permutation to each pixel of the input, the model will finally end up with the same classifier, which is not following our intuition because, as for an image, the order of each pixel does matter.
That is the reason for developing a new type of network for capturing local information.

## A MLP for local information
Naively, we can construct an MLP to capture the local information for an image.

Suppose we have two data representation H, X and U which are both matrix with the same size. H is the new representation
of X which includes local information and U is the bias. And we have a weight matrix W for the whole MLP. Then we have:

{% raw %}
$$\begin{aligned}
H_{ij} &= U_{ij} + \sum_{k}\sum_{l} W_{ijkl} X_{kl}\\
&= U_{ij} + \sum_{a}\sum_{b} W_{ij(i+a)(j+b)} X_{(i+a)(j+b)}
\end{aligned}$$
{% endraw %}

We construct the new representation H at i, j  with the sum of local information at area kl for X. Since
the local information usually around the same position ij, we replace k = i+a, l = j+b.

Now, let's focus on the number of parameters that this MLP contains. Suppose the input image is 1000*1000, the
W matrix will have 10^12 parameters. It is not practical at all.

##  Constraining the MLP
### Translation Invarience
So far, we know that the weight matrix W is the fourth-order tensor, with subscripts i,j,k,l. Since the way we
capture local information should be the same, just like we use the same eye to scan each part of the image, the i, j
is not necessary. It also means that (i,j) has nothing to do with the way we get the local information. Therefore:

{% raw %}
$$\begin{aligned}
H_{ij} &= U_{ij} + \sum_{a}\sum_{b} W_{ab} X_{(i+a)(j+b)}\\
\end{aligned}$$
{% endraw %}

With this constrain, W matrix will have only 10^6 parameters which is way better than the perious MLP. But we still have
dependency on a, b (-1000,1000).

### Locality
Intuitively, a, b is the size of the local area that we care about for each inspection. Therefore, we need to constrain
a and b for a smaller local area. Say a, b are in (-s, s), Therefore, we have:

{% raw %}
$$\begin{aligned}
H_{ij} &= U_{ij} + \sum_{a=-s}^{s}\sum_{b=-s}^s W_{ab} X_{(i+a)(j+b)}\\
\end{aligned}$$
{% endraw %}

Now we have reduced the number of parameters from 4 * 10^6 to 4 * s^2. The s is usually no larger than 10.

## Why named Converlution?

In mathematics, the converlution between two function f and g is :

{% raw %}
$$\begin{aligned}
(f * g) (x) =  \int f(z)g(x - z) dz\\
\end{aligned}$$
{% endraw %}

The discreat form is that:

{% raw %}
$$\begin{aligned}
(f * g) (i) =  \sum_{a} f(a)g(i - a)\\
\end{aligned}$$
{% endraw %}

For two-dimentional tensor:

{% raw %}
$$\begin{aligned}
(f * g) (i,j) =  \sum_{a}\sum_{b} f(a,b)g(i - a, j-b)\\
\end{aligned}$$
{% endraw %}

## Reference
Please reach out to this material for more explaination.

Zhang, A., Lipton, Z.C., Li, M., & Smola, A.J. (2021). Dive into Deep Learning. Release 0.8.0. D2L.ai. http://d2l.ai/
(p. 243-245)
---
layout: post
title:  "Test Everything Blog"
date:   2023-04-02
categories: jekyll update
toc: true
img-url: 2023-04-02-test-everything-blog\
---


* TOC
{:toc}

## 1 (A lv2 header)
### 1.1(A lv3 header) test css "blog-image"
<div class = "blog-image">
    <div>
        <img src="{{ site.blog-img-url }}{{ page.img-url }}test-1.png">
    </div>
    <div>
        <legend>This is a discription for this image.</legend>
    </div>
</div>

### 1.2 test css "blog-video"
<div class = "blog-video">
    <video  controls>
        <source src="https://drive.google.com/uc?export=download&id=1WafTItDAKgdysiMkJcpHL7TzRup1Ztjk" type='video/mp4'>
    </video>
</div>

### 1.3 test LaTex formular.

{% raw %}
$$x^2 + y^2 = r^2$$
{% endraw %}

### 1.4 test complax LaTex formular.

{% raw %}
$$\begin{aligned}
D_{KL}(f(x)||g(x)) &= \int f(x) \log(\frac{f(x)}{g(x)})dx\\
&= - \int f(x) \log(\frac{g(x)}{f(x)}) dx\\
& \ge - \int f(x) (\frac{g(x)}{f(x)} - 1) dx\\
&= - \int g(x) - f(x) dx\\
&= 0 
\end{aligned}$$
{% endraw %}
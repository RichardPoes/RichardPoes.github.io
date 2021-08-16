---
layout: post
title: Creating a math blog
date: 2021-08-04 16:53:20 +0300
description: Starting a (math) blog, how did I approach this?
img: Blog1.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [GitHub Pages, Computers, Blogs]
use-math: true
---
# Introduction
As many people, I wanted to create a mathematics blog.
I decided that GitHub Pages would be perfect, as this should be highly customizable, while also not being intensive on the HTML and CSS work.
This post is divided into two parts: installation and customization, describing how I got this blog in its current shape.

# Installation
First I followed the tutorial on [GitHub Pages](https://guides.github.com/features/pages/ "GitHub Pages Tutorial") to initialize my repository.
After some playing around I found [this template](https://github.com/artemsheludko/flexible-jekyll "GitHub Pages Template").
I cleared my whole repository and filled it with the unzipped files of the aforementioned template. It is then important to set 
``` yaml
baseurl: "" # the subpath of your site, e.g. /blog
```
Because I did not have this subpath by default; this had to be directly on [richardpoes.github.io](richardpoes.github.io), for now.
The site was showing the same content as the template now, so the installation was done.

# Customization
Then I started customizing the site to my needs.
First I changed all the assets in `assets/img/favicon` to the correct assets, showing my picture.
I used the opportunity to also add some extra favicons necessary for Android and iOS.\
Furthermore I removed all the blog posts inside the `_posts/` folder to show my own posts.
Next, I wanted to be able to render mathematics. 
This is where the annoyance began.
I wanted to be able to render
\\[ A = \begin{pmatrix} a_{11} & \dots & a_{n1} \\\\ \vdots & \ddots & \vdots \\\\ a_{1m} & \dots & a_{nm} \end{pmatrix}, \\]
without a hassle, that is, without escaping too much characters, staying true to the \\(\LaTeX\\) syntax.
However, I had to write
``` latex
\\[ A = \begin{pmatrix} a_{11} & \dots & a_{n1} \\\\ \vdots & \ddots & \vdots \\\\ a_{1m} & \dots & a_{nm} \end{pmatrix}, \\]
```
It turned out the problem was caused by the line
```
{% raw %}{{content | markdownify}}{% endraw %}
```
inside `post.html`.
Which had to be replaced by
```
{% raw %}{{content}}{% endraw %}
```
Which fixed the issue indeed.
This was suggested by [@chrisyeh96](https://github.com/chrisyeh96).
The last thing I want to add is some custom CSS and dark theme support, but this will be added later on and a post about this will follow.
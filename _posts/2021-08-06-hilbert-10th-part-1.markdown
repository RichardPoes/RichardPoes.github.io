---
layout: post
title: Hilbert's 10<sup>th</sup> Problem - Part 1
date: 2021-08-14 19:12:20 +0300
description: An introduction to Hilbert's 10th Problem and Diophantine equations
img: Blog2.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Mathematics, Diophantine]
use-math: true
---
# Introduction
I will be posting a series of posts concerning Hilbert's 10<sup>th</sup> Problem, or in short Hilbert's 10<sup>th</sup>.
This was the topic of my Bachelor thesis and I think a wider audience should hear about the topic and its proof.
My aim is to split this topic into three parts, where this part introduces you to Diophantine equations and why the question was posed.
A second post will then introduce you to the fundamentals of computability theory and the last post will actually demonstrate how the final answer to Hilbert's 10<sup>th</sup> was reached.

## Problem statement
One might wonder what is Hilbert's 10<sup>th</sup> Problem exactly.
In 1900 famous Mathematician [Davild Hilbert][Hilbert] published a [list][23Problems] of 23 mathematical problems, a sort of 20<sup>th</sup> century equivalent of the [Millennium Problems][Milennium Problems].
The 10<sup>th</sup> problem on that list was stated by Hilbert as follows.
> Given a Diophantine equation with any number of unknown quantities and with rational integral numerical coefficients: To devises a process according to which it can be determined by a finite number of operations whether the equation is solvable in rational integers.
{: title="Hilbert's statement of his 10<sup>th</sup> problem"}
In 1900, no notion of _algorithm_ had been established yet, which explains the quite cumbersome combination of the words "a process" and "finite number of operations", where the word operations is never defined.
In modern wording we could paraphrase the Diophantine problem as follows:
> Construct an algorithm which takes as input an arbitrary Diophantine equation and outputs whether or not there exists a solution to this equation.
{: title="Paraphrasing of Hilbert's 10<sup>th</sup> problem"}
Note that two key concepts are necessary to understand: the notion of a Diophantine equation and the notion of an algorithm.
The first will be explained in this post and the latter will be explained in Part 2.

# Diophantine equations
To see why this question is both interesting and hard let us explore some simple equations.
What it means to be Diophantine will be explained along the way.
Consider the following equation:

$$ 3x + 2y = 1 $$

Normally it is quite easy to solve an equation like that. 
You can try it out yourself, or view the spoiler.
<details markdown="block"><summary class="solution-summary">Solution</summary>

$$
\begin{align*}
3x + 2y &= 1\\
2y &= 1 - 3x\\
y &= \frac{1}{2} - \frac{3}{2}x
\end{align*}
$$

</details>
Thus we can pick any $$x \in \mathbb{R}$$ and we can find an according $$y$$ such that the equation holds.
However, what if we wanted that both $$x, y$$ are integers?
Things get a lot harder then.
You could try out different pairs of $$(x, y)$$ and see what works.
Or you could look at the solution in $$\mathbb{R}$$ and find the values for $$x \in \mathbb{Z}$$ such that also $$y \in \mathbb{Z}$$.
Try to find a general solution, or look at the spoiler.
<details markdown="block"><summary class="solution-summary">Solution</summary>
When $$x$$ is a multiple of 2, observe that $$-\frac{3}{2}x$$ is an integer.
However, since we add $$\frac{1}{2}$$ to it, $$y$$ will never be an integer.
When $$x$$ is _not_ a multiple of 2, $$y$$ is guaranteed to be an integer.
Thus $$x = 2n + 1$$ for any $$n \in \mathbb{Z}$$.
Filling this in we get that 

$$
\begin{align*}
y &= \frac{1}{2} - \frac{3}{2}(2n + 1)\\
y &= \frac 1 2 - 3n + \frac 3 2\\
y &= 2 - 3n
\end{align*}
$$

Thus the Diophantine equation $$ 3x + 2y = 1 $$ has solutions $$(2n + 1, 2 - 3n)$$, where $$n \in \mathbb{Z}$$ .
</details>
We are now ready for a formal definition.

<span class="definition">Definition 1 (Diophantine equation)</span> A <span class="keyword-definition">Diophantine equation</span> is a polynomial equation that can be written as 

$$
f(x_1, x_2, \dots, x_n) = 0,
$$

where $$f$$ is a polynomial $$a_nx^n + a_{n-1}x^{n-1} + \dots a_1x + a_0$$, such that each $$a_i$$ is an integer.
The polynomial $$f$$ is then referred to as a <span class="keyword-definition">Diophantine polynomial</span>.

## Examples
This definition is quite abstract, so some examples are given.

<span class="example">Example 1</span> The aforementioned equation $$ 3x + 2y = 1 $$ is Diophantine, because it can be rewritten to $$ 3x + 2y - 1 = 0 $$. We now know that this equation has a solution.

<span class="example">Example 2</span> This above example then generalised to $$ax + by = 1 $$, where $$a, b \in \mathbb{Z}$$. This is a [linear Diophantine equation][linearDiophantine] and has a solution exactly when the [greatest common divisor][gcd] $$\operatorname{gcd}(a, b) = 1$$.

<span class="example">Example 3</span> Another interesting example is

$$ x^2 = a, $$

for arbitrary positive integer $$a$$.
Clearly $$a$$ needs to be a perfect square.
But how does one determine whether an integer $$a$$ is a perfect square? 
For low numbers this is easy, but is 3,751,969 a perfect square?
This is not straightforward at all and it is here that we find different _algorithms_ to determine so.
Looking on the internet there are already multiple approaches to this problem, two of which can be found [here][approach1] and [here][approach2].

<span class="example">Example 4</span> The equation
$$x^3 + y^3 + z^3 = 3$$
is also Diophantine. 
This equation is interesting because it has been conjectured to have an infinite set of solutions, yet only two have been found.
Those are $$(1, 1, 1)$$ and $$(569\ 936\ 821\ 221\ 962\ 380\ 720, -569\ 936\ 821\ 113\ 563\ 493\ 509, -472\ 715\ 493\ 453\ 327\ 032)$$.
The second triple is somewhat larger than the first one and was quite hard to find, a [Numberphile video][numberphile] was made in celebration of finding this number.
This problem is referred to as the problem of [sums of three cubes][three cubes].

# The need for a general solution
We have seen that Diophantine problems tend to get quite hard when we introduce higher powers or more variables. 
Furthermore, every new 'family' of Diophantine problems demands a new approach towards the solution. 
It was therefore that Hilbert asked for a general approach to solve all of them, he had hoped for a hidden commonality.
The search for a solution took seventy years and Yuri Matiyasevich laid the final piece of the puzzle in 1970, relying heavily in the work of Martin Davis, Hilary Putnam and Julia Robinson.

It turned out however, that this problem was _incomputable_: that means that a solution cannot exist. 
What it means to be incomputable will be explained in the next post, where we will also explore some computability theory.
In the last post we will further explain how this was proven.

[Hilbert]: https://en.wikipedia.org/wiki/David_Hilbert
[23Problems]: https://en.wikipedia.org/wiki/Hilbert's_problems#Table_of_problems
[Milennium Problems]: https://www.claymath.org/millennium-problems
[linearDiophantine]: https://en.wikipedia.org/wiki/Diophantine_equation#Linear_Diophantine_equations
[gcd]: https://en.wikipedia.org/wiki/Greatest_common_divisor
[approach1]: https://www.urbanpro.com/class-ix-x-tuition/fastest-way-how-to-check-if-a-number-is-a
[approach2]: https://www.geeksforgeeks.org/check-if-given-number-is-perfect-square-in-cpp/
[numberphile]: https://www.youtube.com/watch?v=GXhzZAem7k0
[three cubes]: https://en.wikipedia.org/wiki/Sums_of_three_cubes
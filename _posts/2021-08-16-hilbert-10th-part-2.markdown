---
layout: post
title: Hilbert's 10th Problem - Part 2
html-title: Hilbert's 10<sup>th</sup> Problem - Part 2
date: 2021-08-16 19:12:20 +0300
description: What is an algorithm mathematically? And why are there incomputable problems?
img: Blog3.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Mathematics, Hilbert's 10th, Computability Theory]
use-math: true
---
# Introduction
In the previous post we fiddled around a bit with Diophantine equations, to understand why they were hard.
In the end it was told that an algorithm which _decides_ whether a given Diophantine equation has a solution, cannot exist.
However, it was never clarified what an algorithm was.
Even stronger, David Hilbert itself did not have a clear notion of an algorithm, hence the cumbersome formulation of his 10<sup>th</sup> problem.
In this post we will therefore explore the realms of computability theory, with the main goal being to get some idea of what an algorithm is and why there are incomputable problems.

# Algorithms
One might recall from the previous post that Hilbert's notion of an algorithm had at least something to do with "a process" and a "finite number of operations".
Thus it is best to define such a process.
This will be done using _register machines_.
Those cannot be rigorously defined, but using the concept of them, we can then define what it means to be a _computation_.
## Register Machines
A <span class="keyword-definition">register machine</span> is basically a very simple computer and thus needs to exhibit two properties: possession of some sort of memory and ways to manipulate that memory.
A register machine with $$n$$ registers (or memory cells) is able to store $$n$$ different natural numbers, a depiction of which is shown below.
![Simple depiction of a register machine](../assets/img/tikz/register-machine.png){:.invert-when-dark}
Thus register $$i$$ is denoted with $$R_i$$ and contains the value $$r_i \in \Naturals. $$
Note that, whereas a normal computer memory cell can only contain values up to a certain upper bound, any register $$R_i$$ can contain an arbitrarily large integer $$r_i$$.

To manipulate these memory cells we need a <span class="keyword-definition">program</span> $$P$$.
A program $$P$$ is an ordered list consisting out of two different kind of <span class="keyword-definition">instructions</span> or <span class="keyword-definition">commands</span>.
Each command has one of two shapes:
1. $$r_i^+ \Rightarrow n,$$ which means: add 1 to $$r_i$$ and move to command $$n$$;
2. $$r_i^- \Rightarrow n, m, $$ which means: if $$r_i > 0,$$ subtract 1 from $$r_i$$ and move to command $$n$$. If $$r_i = 0$$, meaning 1 cannot be subtracted, move to command $$m$$.

Let us now dive into some examples.

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
Those are $$(1, 1, 1)$$ and 

$$
\begin{align*}
x &= 569\ 936\ 821\ 221\ 962\ 380\ 720,\\
y &= -569\ 936\ 821\ 113\ 563\ 493\ 509\\
z &=  -472\ 715\ 493\ 453\ 327\ 032.
\end{align*}
$$

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
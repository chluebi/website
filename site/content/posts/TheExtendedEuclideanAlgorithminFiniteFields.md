---

title: "The Extended Euclidean Algorithm in Finite Fields"

date: 2023-02-13

draft: false

  
tags: ["featured", "mathematics", "discmath"]
---
*This article has been adapted from an earlier PDF I wrote.*

#### Motivation
Given that several operations in discrete mathematics require one to find the inverse of integers or polynomials in finite fields, it is important to learn an efficient algorithm to do so quickly. The Extended Euclidean Algorithm is the most primitive of these algorithms and essential for students.

In this article, I will explain use this algorithm on a few example problems, hopefully giving some intuition to future students.


#### Standard Euclidean Algorithm

Let us find the gcd of $144$ and $54$:

At first, we just use the standard Euclidean algorithm, making sure to write down every step as follows:

Take $1 \cdot 144$ subtracted by the largest $k \in\mathbb{N}$ such that $k \cdot 54 < 144$:
$$(1)(144) - (2)(54) = 36$$
Now take $1 \cdot 54$ subtracted by the largest $k \in\mathbb{N}$ such that $k \cdot 36 < 54$
$$(1)(54) - (1)(36) = 18$$
Continue until you reach $0$ on the right-hand side
$$(1)(36) - (2)(18) = 0$$
It is guaranteed that the Euclidean Algorithm reaches the result of $0$ after a finite number of steps.

#### Extended Euclidean Algorithm to find the gcd
Now what the extended Euclidean algorithm allows us to do is to find the gcd of the two numbers as a sum of the two times certain factors. In other words, we want to find coefficients $a, b \in \mathbb{Z}$ such that:
$$a \cdot 144 + b \cdot 54 = \textrm{gcd}(144, 54) = 18$$

For this we go *backwards* through the steps of the Euclidean Algorithm (we ignore the step where we get $0$ as this step is irrelevant to this.)

$$18 = (1)(54) - (1)(36)$$
Now we insert the expression we had in the previous equation for $36$:
$$18 = (1)(54) - (1)((1)(144) - (2)(54))$$
Now we simplify, whilst always keeping in mind that we are interested in the factors of $54$ and $144$, so we treat these two numbers like variables:
$$18 = (1)(54) - (1)((1)(144) - (2)(54))$$
$$= (1 - 1 \cdot (-2))(54) + (-1 \cdot 1)(144)$$
$$= (3)(54) + (-1)(144) = gcd(144, 54) = 18$$

And therefore we have found the needed factors.


#### Extended Euclidean Algorithm for finding multiplicative inverses

Given the following modulo-equation:
$$18x \equiv_{41} 1$$
This is clearly solvable as $gcd(18, 41) = 1$

We know this is equivalent to:
$$\iff \exists k_{\in \mathbb{Z}} (18x - 41k = 1)$$

Notice how this problem simplifies to the same as the one in the previous chapter: we are also looking for factors $x, k$ such that
$$18x - 41k = \textrm{gcd}(18, 41) = 1$$

Therefore, we can use the Extended Euclidean algorithm:
$$(1)(41) - (2)(18) = 5$$
$$(1)(18) - (3)(5) = 3$$
$$(1)(5) - (1)(3) = 2$$
$$(1)(3) - (1)(2) = 1$$
$$(1)(2) - (2)(1) = 0$$

Now solving for $1$:
$$1 = (1)(3) - (1)(2)$$
Inserting for $2$:
$$= (1)(3) - (1)((1)(5) - (1)(3))$$
$$= (2)(3) + (-1)(5)$$
Inserting for $3$:
$$= (2)((1)(18) - (3)(5)) + (-1)(5)$$
$$= (2)(18) + (-7)(5)$$
Inserting for $5$:
$$= (2)(18) + (-7)((1)(41) - (2)(18))$$
$$= (16)(18) + (-7)(41)$$

Therefore,
$$18 \cdot 16 \equiv_{41} 1$$
and
$$18^{-1} \equiv_{41} 16$$


#### A note on coefficients

Most often it is obvious by what factor you have to multiply the smaller amount to subtract it from the larger amount, as in the previous example:
$$(1)(41) - (2)(18) = 5$$

But with larger and larger numbers (and also with polynomials), it might not become as obvious. This when for each step one can execute a division algorithm of choice with rest to find the given coefficient and rest:

Example: $3130$ and $51$

$$3130 : 51 = \frac{3111}{51} + \frac{19}{51} = 61 + \frac{19}{51}$$

Therefore, our first step will be
$$(1)(3130) - (61)(51) = 19$$

And then we continue the algorithm as normal


#### Polynomials

This exact same algorithm can be used for polynomials, it just gets computationally more heavy due to the added complexity of polynomials.

Let us find the inverse of $(x^2+1)$ in the field $GF(2)_{x^4 + x^3 + 1}$.

Therefore, we want to solve the equation
$$p(x^2+1) \equiv_{x^4 + x^3 + 1} 1$$
In other words:
$$\iff p(x^2+1) - q(x^4 + x^3 + 1) = 1$$

Now notice the following:
If we ever get an equation of this form:
$$\iff a(x^2+1) + b(x^4 + x^3 + 1) = c$$
Where $c$ is a number (or a polynomial of zeroth order)

We can get
$$\iff  c^ {-1}a(x^2+1) + c^ {-1}b(x^4 + x^3 + 1) = 1$$
$$\iff  c^ {-1}a(x^2+1) - (-c^ {-1}b)(x^4 + x^3 + 1) = 1$$
$$\iff p(x^2+1) - q(x^4 + x^3 + 1) = 1$$
with $p = c^ {-1}a$ and $(-c^ {-1}b)$


The big "top-level" idea therefore is to run the algorithm as many steps as needed until the rest is a polynomial of zeroth order $c$.

Then we solve for $c$ like in the above sections until we get an equation of the form
$$\iff a(x^2+1) + b(x^4 + x^3 + 1) = c$$
And then turn this equation into a form
$$\iff p(x^2+1) - q(x^4 + x^3 + 1) = 1$$
from which we can read the inverse of $x^2 + 1$ as $p$.


Let us start the Euclidean algorithm:
$$(1)(x^4 + x^3 + 1) - (u)(x^2+1) = r$$

Now we use polynomial division to find $u$ and $r$:
(Remember that we are in GF(2), therefore $-1 = 1$)

$$\frac{x^4+x^3+1}{x^2+1} = \frac{x^4+x^2}{x^2+1} + \frac{x^3 - x^2 + 1}{x^2+1} = x^2 + \frac{x^3+x^2+1}{x^2+1}$$
$$\frac{x^3 + x^2 + 1}{x^2+1} = \frac{x^3 + x}{x^2+1} + \frac{x^2 - x + 1}{x^2+1} = x + \frac{x^2 + x + 1}{x^2+1}$$
$$\frac{x^2 + x + 1}{x^2+1} = \frac{x^2 + 1}{x^2 + 1} + \frac{x}{x^2 + 1}$$
$$\Rightarrow \frac{x^4+x^3+1}{x^2+1} = x^2 + x + 1 + \frac{x}{x^2+1}$$

Therefore:
$$(1)(x^4 + x^3 + 1) = ((x^2 + x + 1)(x^2+1) + x)$$
$$(1)(x^4 + x^3 + 1) - ((x^2 + x + 1)(x^2+1) + x) = 0$$
$$(1)(x^4 + x^3 + 1) - (x^2 + x + 1)(x^2+1) - x = 0$$
Giving us the next step:
$$(1)(x^4 + x^3 + 1) - (x^2 + x + 1)(x^2+1) = x$$
And with the next step we reach a step where we have something $= 1$
$$(1)(x^2 + 1) - (x)(x) = 1$$


And now we solve for $1$:
$$1 = (1)(x^2 + 1) - (x)(x)$$
Inserting for $x$:
$$= (1)(x^2 + 1) - (x)((1)(x^4 + x^3 + 1) - (x^2 + x + 1)(x^2+1))$$
$$= (1 - x(x^2 + x + 1))(x^2+1) + (x)(x^4 + x^3 + 1)$$
$$= (1 - x^3 - x^2 - x)(x^2+1) + (x)(x^4 + x^3 + 1)$$
$$= (x^3 + x^2 + x + 1)(x^2+1) + (x)(x^4 + x^3 + 1)$$
Therefore:
$$q(x^4 + x^3 + 1) - p(x^2+1) = 1$$
$$\iff (x)(x^4+x^3+1) - (x^3 + x^2 + x + 1)(x^2+1) = 1$$
$$\iff (x^3 + x^2 + x + 1)(x^2+1) - (x)(x^4+x^3+1) = 1$$
$$\iff (x^3 + x^2 + x + 1)(x^2+1) \equiv_{x^4 + x^3 + 1} 1$$

Therefore, the inverse of $x^2 + 1$ in the field $GF(2)_{x^4 + x^3 + 1}$ is $x^3 + x^2 + x + 1$.


### Some shorter examples with edge cases
Notice that both of these examples get us a rest of zeroth order after only a single iteration of the euclidean algorithm, which then means that the "solving" part is trivial.

###### Example
Inverse of $x+2$ in $GF(3)[x]_{x^2 + 2x + 1}$

Euclidean algorithm:
$$(1)(x^2 + 2x + 1) - (u)(x+2) = r$$
$$\iff (1)(x^2 + 2x + 1) - (x)(x+2) = 1$$
Now getting it in the form of $p(x+2) - q(x^2+2x+1) = 1$
$$\iff (-x)(x+2) - (-1)(x^2 + 2x + 1) = 1$$
$$\iff (2x)(x+2) - (2)(x^2 + 2x + 1) = 1$$
Therefore the inverse of $x+2$ is $2x$.


###### Example

Inverse of $2x+1$ in $GF(7)[x]_{x^2 + x + 1}$

Euclidean algorithm:
$$(1)(x²+x+1) - (u)(2x+1) = r$$
Euclidean Division gives:
$$(1)(x²+x+1) - (4x+2)(2x+1) = 6$$
Now getting it in the form of $p(2x+1) - q(x^2+x+1) = 1$:
$$(-(4x+2))(2x+1) - (-1)(x^2 + x + 1) = 6$$
We multiply both sides with $6^{-1} = (-1)^{-1} = -1$
$$(4x+2)(2x+1) - (1)(x^2 + x + 1) = 1$$

Therefore, the inverse of $2x+1$ is $4x + 2$.


###### Example

Inverse of $x+4$ in $GF(5)[x]_{x^2 + 1}$

Euclidean algorithm:
$$(1)(x²+1) - (u)(x+4) = r$$
Euclidean Division gives:
$$(1)(x²+1) - (x+1)(x+4) = 2$$
Now getting it in the form of $p(x^2+1) - q(x+4) = 1$:

We multiply both sides with $2^{-1} = 3$
$$(3)(x²+1) - (3x+3)(x+4) = 1$$
$$- (3x+3)(x+4) + (3)(x²+1) = 1$$
$$(-3x-3)(x+4) - (-3)(x²+1) = 1$$
$$(2x+2)(x+4) - (2)(x²+1) = 1$$
Therefore, the inverse of $x^2 + 1$ is $2x+2$.

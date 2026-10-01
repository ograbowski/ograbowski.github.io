---
layout: post
title: "How many gears spin the same way as the first one?"
---

## The problem
> Let's say we have $N$ gears in a row. Each gear meshes only with its neighbors, the one before it and the one after it.
> What's the formula for the number of gears that spin in the same direction as the first one?

## First attempt
The first thought might be:

> A gear can spin in one of two directions, and the next one always spins the opposite way. So I will try $\frac{N}{2}$.

Let's sketch out the gears with their directions of spin to get a clearer view of the problem. If we plug in successive values of $N$, we quickly notice that $\frac{N}{2}$ works only in half of the cases, when $N$ is even.

Why is that? Gear 2 spins the opposite way to gear 1, gear 3 the same way as gear 1, gear 4 the opposite way again, and so on. So the gears spinning with the first one are the ones at odd positions: 1, 3, 5, ... We are really just counting odd numbers from $1$ to $N$. For even $N$ that's exactly half of them. For odd $N$ the last gear is also at an odd position, so we get $\frac{N+1}{2}$ of them.

When $N$ is odd, $\frac{N}{2}$ is never a whole number, and to get the right answer we always have to add $\frac{1}{2}$ to it. So we need a formula that works for both even and odd $N$. For now we write down that $\frac{N}{2}$ works for even $N$ and move on.

## The missing piece
We have to find an expression that:

1. Equals $0$ when $N$ is even, because we already have the formula for that case.
2. Equals $\frac{1}{2}$ when $N$ is odd, so the result gets rounded up to the next integer.

## Building the formula
### 1. Defining a sequence
Let's define a sequence $a_N$, where $a_N$ is the number of gears spinning in the same direction as the first one, when there are $N$ gears in total.

$a_N = \frac{N}{2} + K$, where $K$ is the expression described above.

### 2. Finding $K$
Something that depends on parity is a power of $-1$:

- if $x$ is even, $(-1)^x = 1$
- if $x$ is odd, $(-1)^x = -1$

Remember, the expression we are looking for has to give $\frac{1}{2}$ when $N$ is odd, which rounds $a_N$ up to the next integer. When $N$ is even, it has to give $0$, because the formula for even $N$ is already correct.

$K = \frac{1-(-1)^N}{4}$ suits us perfectly. It gives that precious $\frac{1}{2}$ when $N$ is odd and $0$ when $N$ is even.

> **Why $(-1)^N$?**
> $(-1)^N$ is always either $1$ or $-1$, no matter how big $N$ is, and which one it is depends only on the parity of $N$. So $1-(-1)^N$ is $0$ for even $N$ and $2$ for odd $N$.
>
> **Why is there a $4$ in the denominator?**
> When $N$ is odd, the numerator equals $2$, but we want $\frac{1}{2}$. Since $\frac{2}{4} = \frac{1}{2}$, we divide by $4$.

### 3. Combining everything into one formula
$a_N = \frac{N}{2} + \frac{1-(-1)^N}{4}$, which can also be written as $a_N = \frac{2N+1-(-1)^N}{4}$.

## A shorter formula
If you know the ceiling function $\lceil x \rceil$ (it rounds $x$ up to the nearest integer), you can simply write $a_N = \left\lceil \frac{N}{2} \right\rceil$. For even $N$ it changes nothing, and for odd $N$ it adds exactly the $\frac{1}{2}$ we were missing.

> For $N = 3$:
> $\left\lceil \frac{3}{2} \right\rceil = \left\lceil 1.5 \right\rceil = 2$, it works.
>
> Second try, $N = 4$:
> $\left\lceil \frac{4}{2} \right\rceil = \left\lceil 2 \right\rceil = 2$, it also works. Nothing to round here.

So the long formula from before is just the ceiling function in disguise. We built $\left\lceil \frac{N}{2} \right\rceil$ from scratch without knowing it, using only $(-1)^N$ and a bit of arithmetic.

## Bonus: gears in a circle
What if we close the row into a ring, so the last gear meshes with the first one? For even $N$ everything works fine. For odd $N$ the last gear spins the same way as the first one, but it touches the first one, so the first one would have to spin both ways at once. The whole thing jams. Same parity trick, just from a different angle.

+++
title = "Euclid Algorithm a Beautiful Way to Compute Gcd"
date = 2020-08-19T17:03:29+08:00
markup = "mmark"
tags = ["Euclid algorithm", "gcd"]
categories = ["algorithms"]
+++

## Euclid's algorithm: a beautiful way to compute the greatest common divisor

### Preface

> The greatest common divisor (GCD) of the two integers $a$ and $b$ is defined to be the largest integer that divides both $a$ and $b$ with no reminder. 

Euclid's algorithm is a beautiful and efficient way to compute the GCD(a,b)



### Algorithm

It is simple in the mathematical form:
$$
GCD(a,b) = GCD(b, a \bmod b)
$$
when $b = 0$ in one recursive process , $a$ is the GCD we want.

For example, $GCD(40, 24) = GCD(24, 40 \bmod 24) = GCD(24, 16) = GCD(16, 8) = GCD(8, 0) = 8$

```javascript
function gcd(a, b) {
	return  b === 0 ? a : gcd(b, a % b);
}
```



### Proofs

This article will introduce two ways to prove the validity of this algorithm

#### #1

First we will prove that $GCD(a, b) = GCD(a-b, b)$, assuming that $a>b$

Suppose $GCD(a, b) = p$, so that we can write $a = k_1 p, b = k_2p$, and $GCD(k_1, k_2) = 1$, that is to say, there is no common divisor in $k_1$ and $k_2$ since $p$ is already the **greatest** common divisor.

To prove $GCD(k_1p, k_2p) = GCD((k_1-k_2)p, k_2p)$, we must prove that $GCD(k_1-k_2, k_2) = 1$. 

Assume there is a common divisor $x$, such that $k_1 - k_2 = mx, k_2 = nx$. Then we can deduce that  $k_1 = mx+k_2 = (m+n)x$, so $GCD(k_1, k_2) = x$, which is contradictory to the fact that $GCD(k_1, k_2) = 1$

Hence we can prove that $GCD(k_1p, k_2p) = GCD((k_1-k_2)p, k_2p)$, in other words, $GCD(a, b) = GCD(a-b, b)$

So we can also say

$$
GCD(a, b) = GCD(a-b, b) = GCD(a-2b, b) = ... GCD(a - \lfloor{\frac{a}{b}}\rfloor b, b) = GCD(a \bmod b, b)
$$

#### #2

Assume $a = bk + c$ , so $c = a \bmod b$, and $c = a-bk$

Given any $d$ such that $d | a, d|b$, we can have $\frac{c}{d} = \frac{a}{d} - k\frac{b}{d}$, since right hand side is a integer, left hand side must be an integer

So every common divisor of two integers $a$ and $b$  is also the divisor of $a \bmod b$. Therefore, 

$$
\forall d , d|a \land d|b \ \Rightarrow \ d|b \land d|(a \bmod b)
$$

Next we can use the similar method to prove the common divisor of $b$ and $a \bmod b$ is also the common divisor of $a$ and $b$ (Hint: given any $d$ such that $d|b$, $d|(a \bmod b)$)

So the set of common divisors of $a,b$ is the same with the one of $b, a \bmod b$. So the greatest common divisor is the same as well



### Remark

There is a extended Euclid algorithms to find the solution of certain equations


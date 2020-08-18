+++
title = "Logarithmic Algorithms to Calculate Fibonacci Numbers From SICP JS Exercise 1.19 "
date = 2020-08-19T00:31:21+08:00
tags = ["SICP", "Fibonacci"]
categories = ["SICP"]
+++

## Logarithmic algorithms to calculate Fibonacci numbers - from SICP-JS Exercise 1.19

### Preface

We all know the Fibonacci sequence

$$
\begin{equation}
Fib(k)= 
\begin{cases}
0 & & (k == 0) \\
1 & & (k == 1) \\
Fib(k-1) + Fib(k-2) & & (k >= 2) 
\end{cases}
\end{equation}
$$

and the Fibonacci number $F_n$ denotes the nth term of this sequence

By the definition we can easily write a recursive function to compute $F_n$

```javascript
function calc_fib(n) {
	return n === 0
	       ? 0
	       : n === 1
	         ? 1
	         : calc_fib(n-1) + calc_fib(n-2);
}
calc_fib(3);
//output: 2
```

But this takes time complexity of $O(n)$, that is to the number of steps to calculate basically grows linearly

Here the article will introduce two ways to computer $F_n$ in $O(\log n)$. The first methods comes from the exercise 1.19 in the book [Structure and Interpretation of Computer Programs — JavaScript Adaptation](https://source-academy.github.io/sicp/index.html), which is the textbook used in CS1101S module of NUS. Before you read this, I suggest know how to compute $n^k$ in $O(\log k)$



### Method #1

This is a constructive way to me. We define a way of transform a pair of numbers $(a, b)$, this transformation needs two parameters, we name $p, q$ here. 

We define that, a pair of numbers $(a,b)$, after the transformation $T(p, q)$, becomes $(a(p + q) + bq, bp + aq)$. 

You may ask: "wait, what, how the * this is related to Fib", pls keep reading

Let $p =0, q = 1$, and you will surprisingly find that $(F_{n}, F_{n-1})$ becomes $(F_{n+1}, F_n)$ after the transformation $T(p,q)$

OK, now at least we know this _is_ related to Fibonacci numbers. In fact after doing $n-1$ times $T(p, q)$ you can obtain $(F_n, F_{n-1})$ from pair $(F_1, F_0)$. BUT, its still $O(n)$.

Now, a lazy but clever person comes up with a good idea so that he could do less work, what if I change the parameter $p, q$ to something else $p', q'$ so that the effect of $T(p', q')$ is the same as using two times of $T(p, q)$ ? That is to say $T^2(p, q) = T(p', q')$

You can use your algebra knowledge to obtain that $p' = p^2 + q^2, q' = 2pq + q^2$.  So if you do $n$ times transformation $T(p, q)$ (suppose $n$ is even), it is equal to do $\frac{n}{2}$ times $T(p', q')$ . Or we could say: $T^n(p, q) = T^{\frac{n}{2}}(p', q')$

Now recall how we do the logarithmic algorithms for exponentiation, we can have following programs

```javascript
function calc_fib(p, q, a, b, k) {
	return k > 0 
	       ? is_even(k)
	         ? calc_fib(p * p + q * q, 
                        2 * p * q + q * q, 
                        a, b, k / 2)
	         : calc_fib(p, q, 
                        b * q + a * (p + q), 
                        b * p + a * q, 
                        k - 1)
	       : b;
}
```

To compute $F_n$, the time complexity is $O(\log n)$



### Method #2

> WARNING: You better know the matrix manipulation before reading this section

 In the method we define such transformation $T(p, q)$. But it is an abstract function, right? Here we are going to give a **REAL** transformation by matrix manipulation.

> WARNING: Please get ready to see a true magic

$$
\left[
\begin{matrix}
F_n & F_{n-1} \\
F_{n-1} & F_{n-2}
\end{matrix}
\right]
\cdot
\left[
\begin{matrix}
1 & 1 \\
1 &0
\end{matrix}
\right]
= 
\left[
\begin{matrix}
F_{n+1} & F_{n} \\
F_{n} & F_{n-1}
\end{matrix}
\right]
$$

So we do $n$  times manipulation and will get $F_{n+1}$

But...remember matrix manipulation has the property of associativity? e.g. $A \cdot B \cdot C = A \cdot (B \cdot C)$, $A, B, C$ all denoted a matrix.

So let $B$ denotes 

$$
\left[
\begin{matrix}
1 & 1 \\
1 & 0
\end{matrix}
\right]
$$

$n$ times transformation is :

$$
\left[
\begin{matrix}
F_n & F_{n-1} \\
F_{n-1} & F_{n-2}
\end{matrix}
\right]
\cdot
B ^ n
= 
\left[
\begin{matrix}
F_{n+1} & F_{n} \\
F_{n} & F_{n-1}
\end{matrix}
\right]
$$

And, in fact we can apply the logarithmic algorithm to matrix exponentiation. So the time complexity becomes $O(\log n)$. Since I haven't got a good way to implement matrix and matrix manipulation in _source_ language, I will not give the code here. 



### Remark

In fact we can find a instance for the transformation in **Method #1**, in matrix form.

$$
\left[
 \begin{matrix}
       0 & 0 \\
       b & a 
\end{matrix}
\right]
\cdot
\left[
\begin{matrix}
       p & q \\
       q & (p+q) 
\end{matrix}
\right]
$$

notice that

$$
\left[
\begin{matrix}
       p & q \\
       q & (p+q) 
\end{matrix}
\right]
\cdot
\left[
\begin{matrix}
       p & q \\
       q & (p+q) 
\end{matrix}
\right]
= 
\left[
\begin{matrix}
       p^2 + q^2 & 2pq +q^2 \\
       2pq +q^2 & p^2 + 2pq+2q^2
\end{matrix}
\right]
=
\left[
\begin{matrix}
       p' & q' \\
       q' & (p'+q') 
\end{matrix}
\right]
$$


 ### Reference

[Interpretation of Computer Programs — JavaScript Adaptation](https://source-academy.github.io/sicp/index.html), 1.2.4, Exercise 1.19





 
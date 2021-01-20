+++
title = "A Greedy Algorithm for Load Balancing"
date = 2021-01-20T18:20:25+08:00
tags = ["Greedy Algorithm", "CS2040S"]
categories = ["Algorithms"]

+++

## Preface

This is from optional problem of problem set 2 of CS2040S course.

> Given $p$ processors and an integer array $\text{jobSize}$[ ]. The array has $m$ jobs and $i \text{th}$ job need $\text{jobSize}[i]$ time for a processor to handle. What is the minimum achievable maximum load on any one processor ?

The problem already stated that it is a NP-hard problem. All we need to do is to find a greedy algorithm to find a "good" solution and to prove this solution is no more than twice of the optimal solution (say it is 2-approximation of optimal).

The problem also gives hint that we only need to assigns each job to the least loaded processor.

I know how to implement this algorithm using heap, but have no idea of proving this property. So I turned to *Introduction of Algorithms* for help and, luckily, found there is an exercise in the chapter *approximation algorithm* which is almost the same! Then I read the chapters in parallel computing chapter and find this theorem. Just amazing!

The exercise problem is exercise 35-5, page 1135 of Introduction to Algorithms 3rd edition and proof is from page 782, Theorem 27.1 and 27.2.



## Algorithm

Heap, the rest omitted

The time complexity is $O((m + p) \log p)$



## Proof

Let $C_{max}$ denote the solution derived from the greedy algorithm, and $C_{opt}$ denote the optimal solution

Let $P_1$ denote the total amount of work, which is $\sum \text{jobSize[]}$

Let $P_{max}$ denote the maximum work load, which is maximum number in the $\text{jobSize}[]$ array

and $p$ is the number of processors



First we need to prove $C_{max} \leq  \lfloor{P_1 / p}\rfloor + P_{max}$

1. Consider the time span during which all $p$ processors are doing a work, say it lasts $T_1$

    Suppose this $T_1$ is greater than $\lfloor{P_1 / p}\rfloor$, say it is  $\lfloor{P_1 / p}\rfloor + 1$ (since we are doing this in an integer system)

    then the amount of work done during this time span is $p \times (\lfloor{P_1 / p}\rfloor + 1) = P_1 - (P_1 \mod p) + p$

    since $(P_1 \mod p) < p$, the amount of work done during the time span is greater than $P_1$ ! which is contradictory !

    Therefore $T_1 <= \lfloor{P_1 / p}\rfloor$

2. Consider the time span during which NOT all $p$ processors are doing a work, say it lasts $T_2$

    It is impossible for $T_2 > P_{max}$ if we assigns each job to the *least loaded processor*

    (not strict proof but I guess it is easy to understand)

Because either all $p$ processors are doing a work or not all $p$ processors are doing a work

$C_{max}$, the longest time span, should be less or equal to the sum of these two time span.

So, $C_{max} \leq  \lfloor{P_1 / p}\rfloor + P_{max}$



Second, it is already given that $C_{opt} >= \lfloor{P_1 / p}\rfloor$ and $C_{opt} >= P_{max}$

it is not hard to deduce that $C_{max} <= 2 \times C_{opt}$




---
title: Polland Rho Algorithm
date: 2018-11-11 13:48:32
categories:
- algorithm
- cryptography
tags:
- Cryptography
- Numerical Algorithms
- Factorization
---

# Introduction

- Pollard's rho algorithm is an algorithm for integer factorization. It was invented by John Pollard in 1975.
- It uses only a small amount of space, and its expected running time is proportional to the square root of the size of the smallest prime factor of the composite number being factorized.

# Description

Assume $g(x)$ is a sequential random number generator.
For example:
- $x\_{1} = g(x\_{0})$
- $x\_{2} = g(x\_{1})$
- ...

The Polland Rho algorithm is

```python
def factor(N):
    x=y=2, d=1
    while d == 1:
        x = g(x)
        y = g(g(y))
        d = GCD(|x-y|, N)
    if d == N:
        return False
    else:
        return d
```

However $g(x)$ may have cycles. We need a cycle detection algorithm, e.g. **the Floyd's cycle-finding algorithm**.

# Variants

- In 1980, Richard Brent published a faster variant of the rho algorithm. He used the same core ideas as Pollard but a different method of cycle detection, replacing Floyd's cycle-finding algorithm with the related Brent's cycle finding method.

# Complexity

The algorithm offers a **trade-off** between its **running time** and the **probability** that it finds a factor. 

A prime divisor can be achieved with a probability around 0.5, in $O(\sqrt{d}) \leq O(n^{\frac{1}{4}})$ iterations. This is a heuristic claim, and rigorous analysis of the algorithm remains open, based on the **Birthday paradox**.
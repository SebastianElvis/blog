---
title: Bilinear Pairing Explained
date: 2018-11-11 16:50:12
categories:
- cryptography
- public-key cryptography
tags:
- Cryptography
- Public-Key Cryptography
- Elliptic Curve 
---

# Definition

- Here are 3 multiplicative cyclic group: $G_{1}$, $G_{2}$ and $G_{T}$.
- Define a mapping $ e: G_{1} \times G_{2} \rightarrow G_{T}$
- If the function $e$ satisfies:

1. **Bilinearity**: $\forall g_{1} \in G_{1}, g_{2} \in G_{2}, a, b \in Z_{p}$, we have $e(g_{1}^{a}, g_{2}^{b}) = e(g_{1}, g_{2})^{ab}$
2. **Non-degeneracy**: $\exists g_{1} \in G_{1}, g_{2} \in G_{2}$, we have $e(g_{1}, g_{2}) \neq 1_{G_{T}}$
3. **Computability**: $e$ can be efficiently computed

We say $e$ is a bilinear pairing on ($G_{1}, G_{2}$)


# Classification

If the same group is used for the first two groups (i.e. $G_{1}=G_{2}$), the pairing is called **symmetric** and is a mapping from two elements of one group to an element from a second group (i.e. $G_{1} \times G_{1} \rightarrow G_{T}$).

Some researchers classify pairing instantiations into three (or more) basic types:

- $G_{1}=G_{2}$
- $G_{1}\neq G_{2}$ but there is an efficiently computable [homomorphism](https://en.wikipedia.org/wiki/Homomorphism) $ \phi :G_{2} \to G_{1}$
- $G_{1}\neq G_{2}$ and there are no efficiently computable homomorphisms between $G_{1}$ and $G_{2}$


# DDH, CDH, BDDH, BCDH

## DDH, CDH

- **DDH (Decisional Diffie-Hellman Problem)**: For aribitrary $a, b, c \in Z_{q}$ and $g$ as the generator in $Z_{q}$, given $g, g^{a}, g^{b}, g^{c}$, decide $c \stackrel{?}{=} ab $ 

- **CDH (Computational Diffie-Hellman Problem)**: Given $ g, g^{x}, g^{y}$, compute $g^{xy} $


Many cryptographic protocols are based on the assumption that DDH and CDH do not have probabilistic polynomial-time (PPT) solutions.

## Problem with Bilinear Pairing

- If $\exists G \times G \rightarrow G_{T}$, DDH can be solved fast.
- Therefore, we need BDDH, which will still be hard if $\exists G \times G \rightarrow G_{T}$

## BDDH, BCDH

- **Bilinear DDH (BDDH)**: For arbitrary $a, b, c, d \in Z_{q}$, given $g^{a}, g^{b}, g^{c}, g^{d}, g^{abc}$, decide $d \stackrel{?}{=} abc$

- **Bilinear CDH (BCDH)**: For arbitrary $a, b, c \in Z_{q}$, given $g^{a}, g^{b}, g^{c}$, compute $g^{abc}$


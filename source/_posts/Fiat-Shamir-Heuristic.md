---
title: Fiat-Shamir Heuristic
date: 2019-03-15 10:26:18
categories:
- cryptography
- zero knowledge
tags:
- Zero Knowledge
---

# Description

The Fiat–Shamir Heuristic [FS86] is a cryptographic technique to create a digital signature of an Interactive Proof of Knowledge and make it publicly verifiable.

This can be used for converting an Interactive Proof of Knowledge to a Non-Interactive Proof of Knowledge.

# Example

## Problem

Here is an example of the Proof of Knowledge (PoK) problem.

- Participants
  - Peggy: the Prover (female)
  - Victor: the Verifier (male)
- Problem
  - Knowledge: Peggy knows $x: y = g^{x}$ where $x \in \mathbb{Z}^{*}_{q}$ (a finite field with the prime degree $q$ and a generator $g$)
  - Problem: Peggy wants to prove she knows $x$ to Victor without revealing $x$

## Solution 1: An Interactive Protocol

1. Peggy picks a random $x \in \mathbb{Z}^{*}_{q}$, then computes $t = g^{v}$, then sends $t$ to Victor
2. Victor picks a random $c \in \mathbb{Z}^{*}_{q}$, then sends it to Peggy
3. Peggy computes $r = v - cx$, then returns $t$ to Victor
4. Victor can check $t \stackrel{?}{=} g^{r} y^{c}$
   - Proof: $g^{x} y^{c} = g^{v-cx} g^{xc} = g^v = t$

Only Victor can verify this, as $c$ is chosen by Victor and others may not trust Victor.

## Solution 2: A Non-Interactive and Publicly Verifiable Protocol

1. Peggy picks a random $x \in \mathbb{Z}^{*}_{q}$, then computes $t = g^{v}$
2. Peggy computes $c = H(g, y, t)$, where $H()$ is an ideal Random Oracle
3. Peggy computes $r = v-cx$, and the resulting Proof of Knowledge is $\pi = (t ,r)$, then she publishes $\pi$
4. $\pi$ can be verifiable by anyone, by checking $t \stackrel{?}{=} g^{r} y^{c}$ ($c$ can be computed from $g, y, t$ by anyone)

Theory:

- $x$ is an entropy source
- $H()$ makes $c$ unpredictable and publicly computable
- $\pi$ can be treated as a digital signature of the Interactive Proof of Knowledge, equivalent to a Non-Interactive proof of Knowledge

# References

- [FS86] [Amos Fiat and Adi Shamir: How to Prove Yourself: Practical Solutions to Identification and Signature Problems. CRYPTO 1986.](https://www.math.uni-frankfurt.de/~dmst/teaching/SS2012/Vorlesung/Fiat.Shamir.pdf)
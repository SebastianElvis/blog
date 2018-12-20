---
title: zkSNARKs Explained
date: 2018-12-19 18:41:18
categories:
- cryptography
- zero knowledge
tags:
- Cryptography
- zkSNARKs
- Zero Knowledge
---

# Introduction

- Zero Knowledge: Verify the correctness of computations without
  - having to execute them
  - learning what was executed
- zkSNARKs
  - **Zero Knowledge - Succinct Non-interactive ARguments of Knowledge** for short
  - **Succinct**: The sizes of msgs are tiny compared to the length of actual computations
  - **Non-interactive**: Little or no interaction
    - In zkSNARKs, only a setup phase + a msg from prover to verifier
  - **ARguments**: Verifier is ONLY protected against computationally limited provers
  - **of Knowledge**: Impossible for prover to construct a proof/argument without knowing *witness*

# Ingredients of zkSNARKs

1. Encoding the program to a quadratic equation of polynomials $t(x)h(x) = w(x)v(x)$
  - The equality holds iff the program is correctly computed
  - The prover wants to convinve the verifier that the equality holds
2. Random Sampling for Succintness
  - Verifier chooses a secret evaluation point $s$
  - To reduce the problem from evaluating polynomials to evaluating $t(s)h(s) = w(s)v(s)$
3. Homomorphic Encoding / Encryption
  - An encoding / encryption function $E$ which is **Partially Homomorphic**
  - Knowing $E(s)$ $\Rightarrow$ Can compute $E(t(s)), E(h(s)), E(w(s)), E(v(s))$
4. Zero Knowledge
   - Prover obfuscates $E(t(s)), E(h(s)), E(w(s)), E(v(s))$ by multiplying with a number
   - Verifier can still check their correct structure without knowing the actual encoded values

# Simple ZK by RSA

- RSA recap:
  - $c = E(m) = m^{e} \mod n$
  - $m = D(c) = c^{d} \mod n$
- RSA is **Multiplicatively Homomorphic**
  - $ E(x)E(y) \equiv x^{e}y^{e} \equiv (xy)^{e} \equiv E(xy) (\mod n) $
- ZK by RSA
  - Prover knows $x, y$ and he wants to prove this for Verifier
  - Prover sends $E(x), E(y), E(xy)$ to Verifier
  - NOT secure!

# NP-Completeness

For any NP problem $L$, there is a Polynomial Time *reduction function* $f$ s.t.:

$ L(x) = SAT(f(x)) $

$f$ can be seen as a **compiler**

## Example

$PolyZero(f) \Rightarrow$ Determine if $f$ is a polynomial which has a zero where its variables are taken from {0, 1}

## Quadratic Span Program (QSP)

- From [Quadratic Span Programs and Succinct NIZKs without PCPs](https://eprint.iacr.org/2012/215.pdf)
- An NP problem suitable for zkSNARKs
- QSP consists of a set of polynomials
- The task is to find a linear combination of those is a multiple of another given polynomial

Formally, a QSP over a field $F$ for inputs of length $n$ consists of:

- Polynomials $v_{0},...,v_{m}$, $w_{0},...,w_{m}$ over the field $F$
- A polynomial $t$ over $F$ (the target polynomial)
- An injective function $f: \{ (i,j) | 1 \leq i \leq n, j \in \{0,1\} \rightarrow \{1,...,m\} \}$

Then

- $ QSP(u) \rightarrow \{0,1\} $
- $u$ is a string input with $n$ bits
- 0 = false, 1 = true
- **Find $u$ is NP-Complete**
  - Find $u$ $\Rightarrow$ Determine factors
  - Determining factors is **NP-Complete**

Formally, an input $u$ is *accepted* (verified) iff there are tuples $ a = (a_{1},...,a_{n}) $, $ b = (b_{1},...,b_{n}) $ from $F$ s.t.

- $a_{k}, b_{k} = 1$ if $k = f(i, u[i])$ for some $i$, ($u[i]$ is the $i$th bit of $u$)
- $a_{k}, b_{k} = 0$ if $k = f(i, 1-u[i])$ for some $i$
- $t \mod VW = 0$ where
  - $V = v_{0} + a_{1}v_{1} + ... + a_{m}v_{m}$
  - $W = w_{0} + b_{1}w_{1} + ... + b_{m}w_{m}$

Note that the verifer can choose a secret random point $s$ to compute $t(s), v_{k}(s), w_{k}(s),...$, then only checks those scalars
  - After used, the point $s$ is part of the "toxic waste" of zCash


# zkSNARKs in Detail

- A trusted setup
  - The circuit (the transaction verifier, or says the **compiler**) is fixed
  - The polynomials for the QSP are fixed
    - Setup once and reused for all TXs
- Verifier chooses a random field element $s$
- Verifier publishes $E(v_{k}(s)), E(w_{k}(s))$
  - Part of the Common Reference String (CRS)
- Prover computes $E(v(s)), E(w(s))$ without knowing $v_{k}(s), w_{k}(s)$

## The Base Protocol

Based on **Knowledge of Exponent (KEA1) Assumption**.

1. Verifier chooses a secret field value $s$ and publishes $E(s^{0}),...E(s^{d})$ as part of CRS
  - $d$ is the max degree of all polynomials
  - After that $s$ is toxic and should be forgotten
  - Now Prover can compute $E(f(s))$ without knowing $s$
2. Verifier chooses another secret field value $\alpha$ and publishes the "shifted" values $E(\alpha s^{0}),...E(\alpha s^{d})$
  - $\alpha$ is also destroyed after the setup phase
  - Proer can also easily compute $E(\alpha f(s))$
3. Prover publishes $A = E(f(s))$ and $B = E(\alpha f(s))$
4. Verifier check that these values match
  - By **Bilinear Pairing**
    - $e(A, g^{\alpha}) = e(g^{f(s)}, g^{\alpha}) = e(g, g)^{\alpha f(s)}$
    - $e(B, g) = e(g^{\alpha f(s)}, g) = e(g, g)^{\alpha f(s)}$
    - If equal, then the value is correct
    - KEA1 assumption assumes coming up with $A, B$ is impossible

## The Base Protocol + ZK Part

In the Base Protocol, Verifier knows the **exact value** of $E(f(s)), E(\alpha f(s))$ from Prover.

Now we want a solution where
- Verifier publishes $E(s^{0}),...,E(s^{d}), E(\alpha s^{0}),...,E(\alpha s^{d})$
- Verifier convinces that Prover knows $E(f(s)), E(\alpha f(s))$
- But Verifier cannot know anything about $f(s)$, not even $E(f(s))$

The solution is as follows:

1. same
2. same
3. Prover picks a random $\delta$, and sends $A' = E(\delta + f(s)), B' = E(\alpha (\delta +f(s)))$
  - e.g. $A' = E(\delta + f(s)) = g^{\delta + f(s)} = g^{\delta}g^{f(s)} = E(\delta)E(f(s))A$


## A SNARK for QSP

We have the following public variables:

- Polynomials $v_{0},...,v_{m},w_{0},...,w_{m}$
- A target polynomial $t$ of degree at most $d$
- An injective function $f: \{ (i,j) | 1 \leq i \leq n, j \in \{0,1\} \rightarrow \{1,...,m\} \}$
- A binary input string $u$

Prover should find 
- $a_{1},...,a_{m},b_{1},...,b_{m}$ (restricted by an injective function $f$ depending on $u$)
- A polynomial $h$

such that

$th = (v_{0}+a_{1}v_{1}+...+a_{m}v_{m}) (w_{0}+b_{1}w_{1}+...+b_{m}w_{m})$

The process is as follows

1. Verifier chooses secret numbers $s$ and $a$, then publishes CRS consisting of
  - $E(s^{0}),...,E(s^{d})$ and $E(\alpha s^{0}),...,E(\alpha s^{d})$
  - Encrypted $t$ values on point $s$
    - $E(t(s))$ and $E(\alpha t(s))$
    - $E(v_{0}(s)),...,E(v_{m}(s))$ and $E(\alpha v_{0}(s)),...,E(\alpha v_{m}(s))$
    - $E(w_{0}(s)),...,E(w_{m}(s))$ and $E(\alpha w_{0}(s)),...,E(\alpha w_{m}(s))$
  - Verifier further chooses secret numbers $\beta_{v},\beta_{w},\gamma$ used for verify that those polynomials were evaluated and not some arbitrary polynomials (formally, **Commitment**)
    - $E(\gamma), E(\beta_{v}\gamma), E(\beta_{w}\gamma)$
    - $E(\beta_{v} v_{1}(s)),...,E(\beta_{v}v_{m}(s))$
    - $E(\beta_{w} w_{1}(s)),...,E(\beta_{w}w_{m}(s))$
    - $E(\beta_{v}t(s)), E(\beta_{w}t(s))$
2. Prover gives *proof* to Verifier
  - Recall $f: \{ (i,j) | 1 \leq i \leq n, j \in \{0,1\} \rightarrow \{1,...,m\} \}$, where $m$ is relatively large
  - $f$ cannot cover all values over $1,...,m$
  - We call values $f$ does not cover $I_{free}$
  - We define $v_{free}(x) = \sum a_{k}v_{k}(x)$ where $k$ ranges over all $I_{free}$
  - The *proof* consists of 
    - $V_{free} = E(v_{free}(s))$, $W = E(w(s))$, $H = E(h(s))$
    - $V'_{free} = E(\alpha v_{free}(s))$, $W' = E(\alpha w(s))$, $H' = E(\alpha h(s))$
    - $Y = E(\beta_{v}v_{free}(s) + \beta_{w}w(s))$
3. Verifier verifies *proof*
  - Verifier computes $E(v_{in}(s)) = E(\sum a_{k}v_{k}(s))$, where $v_{in}$ of Verifier should $= v_{free}$ of Prover
  - Verifier verifies the following equalities using the Pairing function $e$
    - $e(V'_{free}, g) = e(V_{free}, g^{\alpha})$, $e(W', E(1)) = e(W, E(\alpha))$, $e(H', E(1)) = e(H, E(\alpha))$
    - $e(E(\gamma), Y) = e(E(\beta_{v}\gamma), V_{free})e(E(\beta_{w}\gamma), W)$
    - $e( E(v_{0}(s))E(v_{in}(s))V_{free}, E(w_{0}(s))W ) = e(H, E(t(s)))$
  - Insight: Pairing allows us to do some limited computation on encrypted values - **arbitrary additions but just a single multiplication**
    - Addition: Adding over Elliptic Curve is homomorphic: $E(a)=aG, E(b)=bG \Rightarrow E(a+b)=(a+b)G=aG+bG=E(a)+E(b)$
    - Multiplication: From pairing, $e(\alpha G, W) = e(G, \alpha W) \Rightarrow $ solve $\alpha W$ unconditionally


# Reference

1. https://chriseth.github.io/notes/articles/zksnarks/zksnarks.pdf
2. http://zerocash-project.org/media/pdf/zerocash-extended-20140518.pdf
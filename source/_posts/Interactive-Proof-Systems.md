---
title: Interactive Proof Systems
date: 2019-03-15 13:18:54
categories:
- cryptography
- foundation
tags:
- Zero Knowledge
---

# Proofs

A proof is a way of convincing a party of a certain claim. 
When talking about proofs, we consider two parties: the **prover** and the **verifier**.
Given an assertion, the prover's goal is to convince the verifier of the assertion's validity, whereas the verifier's objective is to accept only a correct assertion.

We focus on the verification process rather than finding the proof.
The verification is relatively easy while finding a proof is hard.

The proof systems have different types:

- Non-Interactive Proof (NP)
- Interactive Proof (IP)
- PSPACE (proved as = IP)
- ...

## Non-Interactive Proof (NP)

$V$ is an NP-verifier for the language $L$ if $V$ is polynomial-time in the length of the first input. Here the following two properties hold:

- **Completeness**: If $x \in L$, $\exists \pi: V(x, \pi) = 1$.
  - For a true assertion $x$ in the language $L$, there is a convincing proof strategy $\pi$ for $V$ to prove $x$ is right.
- **Soundness**: If $x \notin L$, $\forall \pi: V(x, \pi) = 0$.
  - For a false assertion $x$ in the language $L$, no convincing proof strategy $\pi$ exists.

In this definition, $\pi$ is a "certificate" for $x$.
Note that because $V$ is polynomial-time in $|x|$, it is necessary that $\pi \leq p(|x|)$, where $p$ is a polynomial.
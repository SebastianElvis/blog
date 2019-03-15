---
title: Merkle Puzzles
date: 2019-02-26 12:17:48
categories:
- cryptography
- public-key cryptography
tags:
- Merkle Puzzles
---

# Description

Merkle Puzzles [Mer75] allow two parties to agree on a shared secret by exchanging messages, even if they have no secrets in common beforehand (similar to Diffie-Hellman Key Exchange).

# Construction

Common Knowledges:

- $n$: the security parameter
- $H: \{0, 1\}^{l} \to \{0, 1\}^{l}$ is a hash function, where $l \gg \log{n}$

The protocol operates as follows:

1. Alice chooses $n$ random numbers $(x_{1}, \dots, x_{n}) \in [n^{2}]$
2. Alice sends $(a_{1}, \dots, a_{n})$ to Bob where $a_{i} = H(x_{i})$ for $i \in [1, n]$
3. Bob chooses $n$ random numbers $(y_{1}, \dots, y_{n}) \in [n^{2}]$
4. Bob sends $(b_{1}, \dots, b_{n})$ to Alice where $b_{i} = H(y_{i})$ for $i \in [1, n]$
5. With at least 0.9 probability, there will be at least one **collision** between Alice's and Bob's messages: a pair $(i, j)$ s.t. $a_{i} = b_{j}$. Alice and Bob choose the lexicographically first such pair as their secrets
   - Alice sets $s_{a} = x_{i}$ as her secret, and Bob sets $s_{b} = y_{j}$ as his secret
   - If no collision, no secret

# References

- [Mer75] [Ralph Merkle. Secure Communication Over Insecure Channels.](http://www.merkle.com/1974/Puzzles1975.12.07.pdf)
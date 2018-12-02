---
title: LATINCRYPT'15 - The Simplest Protocol for Oblivious Transfer
date: 2018-12-01 14:04:51
categories:
- paper reading
tags:
- Oblivious Transfer
- MPC
---




# Info

- **Title**: The Simplest Protocol for Oblivious Transfer
- **Conference**: LATINCRYPT
- **Year**: 2015
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2015/267.pdf
- **Source code**: No

# Addressed Problems

- Oblivious Transfer (OT) is one of the fundamental building blocks of cryptographic protocols.

# Solution

- In this paper we present a *novel* and *extremely simple*, *efficient* and *secure* OT protocol. 
    - 1-out-of-2 random OT
    - The protocol is a simple tweak of the celebrated Diffie- Hellman (DH) key exchange protocol.
    - Our choice for the group is a *twisted Edwards curve* for building the *Ed25519* signature scheme
- We compares the speed of our implementation of $({2 \atop 1})$-OT (i.e., $n=2$) with other similar implementations.

# Methodology

## Twisted Edwards Curve

### The Group $\mathbb{F}$ 

The Twisted Edwards Curve: is defined as 
\[ \{(x,y) \in \mathbb{F}_{2^{255}-19} \times \mathbb{F}_{2^{255}-19} : -x^{2} + y^{2} = 1 + dx^{2}y^{2} \} \]

The Twisted Edwards Addition Law is

\[ (x_{1},y_{1}) + (x_{2},y_{2}) = (\frac{x_{1}y_{2}+x_{2}y_{1}}{1+dx_{1}x_{2}y_{1}y_{2}}, \frac{y_{1}y_{2}+x_{1}x_{2}}{1-dx_{1}x_{2}y_{1}y_{2}}) \]

### Encoding $\mathcal{E}$ of group elements

- An encoding $\mathcal{E}$ for a group $\mathbb{G}_{0}$ is a way of representing group elements as fixed-length bitstrings.
- We write $\mathcal{E}(P)$ for a bitstring which represents $P \in \mathbb{G}_{0}$.
- *Note* that there can be multiple bitstrings that represent $P$
    - If there is only one bitstring for each group element, $\mathcal{E}$ is said to be *deterministic* 
    - $\mathcal{E}$ is said to be non-deterministic otherwise.
- *Note* that some bitstrings (of the fixed length) might **not** represent any group element.
- We write $ \mathcal{E}(\mathbb{G}_{1}) $ for the set of bitstrings which represent some element in $ \mathbb{G}_{1} \in \mathbb{G}_{0} $. 
- $\mathcal{E}$ is said to be **verifiable** if there exists an efficient algorithm that, given a bitstring as input, outputs whether it is in $E(G0)$ or not.

## Diffie-Hellman Key Exchange

![](dh.png)

## Proposed OT scheme

![](ot.png)

# Relevance

- OT is a strong primitive to protect the privacy of the message selector
- It may be utilized to improve the Blockchain privacy

# To discuss/investigate

- Implement or benchmark this protocol?
- Any relationship to the NIZK?
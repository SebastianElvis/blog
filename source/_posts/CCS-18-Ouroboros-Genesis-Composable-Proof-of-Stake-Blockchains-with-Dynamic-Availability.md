---
title: >-
  CCS'18 - Ouroboros Genesis: Composable Proof-of-Stake Blockchains with Dynamic Availability
date: 2018-11-08 19:15:19
categories:
- paper reading
tags:
- CCS
- Cryptography
- Blockchain
- Consensus
- PoS
---


# Info

- **Title**: Ouroboros Genesis: Composable Proof-of-Stake Blockchains with Dynamic Availability
- **Conference**: ACM CCS
- **Year**: 2018
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2018/378.pdf
- **Source code**: No

# Simplified Definitions

- **Dynamic availability**: tolerating the *Churn* problem
- **Composability**: Miners can double the value by mining two coins with the same PoW protocol simultaneously.

# Addressed Problems

- **None** of the existing PoS-based blockchain protocols can realize the *ledger functionality* under *dynamic availability* in the same way that bitcoin does (using only the information available in the genesis block).
  - PoS is unknown to tolerate the *composability* problem
  - PoS protocols often severely restrict the *dynamic availability*.
- PoW has good *dynamic availability* but has poor *efficiency* and has *composability* issues.


# Solution

- This paper provides a *Universally Composable (UC)* treatment of PoS-based blockchains.
  - The treatment is in the UC model with global setups, a.k.a. GUC. Note that all statements trivially apply also to the standard UC model by considering **global setups as ideal UC functionalities**.
  - The treatment meets the *GUC* and *Dynamic Availability* requirements
- Formalized Global UC
- Formalized Dynamic Availability


# Methodology

# Relevance

PoS is one of the solution to scale public blockchains, while the public blockchian scalability is our main research interest.

# To discuss/investigate

Ouroboros series mainly focus on the foundation/theory of distributed consensus. We may need to understand and utilize these definitions/primitives better.
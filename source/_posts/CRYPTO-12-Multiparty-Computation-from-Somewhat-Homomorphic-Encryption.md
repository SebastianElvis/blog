---
title: CRYPTO'12 - Multiparty Computation from Somewhat Homomorphic Encryption
date: 2018-12-02 14:09:14
categories:
- paper reading
tags:
- MPC
- SPDZ
- Homomorphic Encryption
- CRYPTO
---


# Info

- **Title**: Multiparty Computation from Somewhat Homomorphic Encryption
- **Conference**: CRYPTO
- **Year**: 2012
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2011/535.pdf
- **Source code**: https://github.com/bristolcrypto/SPDZ-2

# Addressed Problems

- Recently, a new approach has been proposed making such protocols more practical. This approach works as follows: one first designs a general MPC protocol in the preprocessing model, where access to a "trusted dealer" is assumed.
    - The dealer does not need to know the function to be computed, nor the inputs, he just supplies raw material for the computation before it starts. 
    - This allows the “online” protocol to use only cheap information theoretic primitives and hence be efficient. F
    - One implements the trusted dealer by a secure protocol using public-key techniques, this protocol can then be run in a preprocessing phase.
- Recently, another approach has become possible with the advent of Fully Homomorphic Encryption (FHE) by Gentry [15]. 
    - The advantage of the FHE-based approach is that interaction is only needed to supply inputs and get output. 
    - However, However, the low bandwidth consumption comes at a price; current FHE schemes are very slow and can only evaluate small circuits, i.e., they actually only provide what is known as somewhat homomorphic encryption (SHE).

# Solution

- We propose an MPC protocol in the preprocessing model that computes securely an arithmetic circuit C over any finite field $\mathcal{F}_{p^{k}}$.
    - The protocol is statistically UC-secure against active and adaptive corruption of up to n−1 of the n players
    - We assume synchronous communication and secure point-to-point channels.
- As a conceptual contribution we propose what we believe is **the right way** to use FHE/SHE for computationally efficient MPC, namely to use it for **implementing a preprocessing phase**.
- We obtain a constant-round preprocessing protocol that is UC-secure against active and static corruption of $n−1$ players assuming the underlying cryptosystem is semantically secure, which follows from the polynomial (PLWE) assumption.
- Both the preprocessing and online phase have been implemented and tested for 3 players on up-to-date machines connected on a LAN.

# Methodology



# Relevance

- FHE may have potential to construct consensus scheme
- However, currently too complex and inefficient...

# To discuss/investigate

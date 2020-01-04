---
title: ASIACRYPT'01 - Short Signatures from the Weil Pairing
date: 2018-12-24 12:45:53
categories:
- paper reading
tags:
- Elliptic Curve
- BLS
- Pairing
- Short Signatures
- Signature Aggregation
- Threshold Encryption
---



# Info

- **Title**: Short Signatures from the Weil Pairing
- **Conference**: ASIACRYPT
- **Year**: 2001
- **URL**: https://www.iacr.org/archive/asiacrypt2001/22480516.pdf
- **Source code**: No

# Addressed Problems

- A 320-bit signature is too long to be keyed in by a human.

# Solution

- We propose a signature scheme whose length is approximately 160 bits and provides a level of security similar to 320-bit DSA signatures.
  - secure against existential forgery under a chosen message attack (in the random oracle model) 
  - Verifying the signature is done using a bilinear pairing on the curve.

# Methodology

1. GEN: private key $x$, public key $g^{x}$
2. SIGN: for message $m$, $h = H(m)$, the signature $\sigma = h^{x}$
3. VERIFY: check $ e(\sigma, g) \stackrel{?}{=} e(h, g^{x}) $

Properties:

1. Simple threshold encryption
2. Signature aggregation with constant signature size
3. Unique & deterministic

# Relevance

- BLS signature is widely used in blockchain

# To discuss/investigate

- Research on BLS-based blockchain protocols?


fuckyou
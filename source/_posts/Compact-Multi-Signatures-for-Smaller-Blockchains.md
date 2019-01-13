---
title: Compact Multi-Signatures for Smaller Blockchains
date: 2019-01-04 19:54:37
categories:
- paper reading
tags:
- MultiSig
- BLS
- Pairing
---


# Info

- **Title**: Compact Multi-Signatures for Smaller Blockchains
- **Conference**: No
- **Year**: 2018
- **URL**: https://eprint.iacr.org/2018/483.pdf
- **Source code**: No

# Addressed Problems

- However, recent work [21] has shown that there is a gap in the security proof, and that security cannot be proven under this assumption. 
- Whether their scheme (MuSig) can be proved secure under a different assumption or in a generic group model is currently an open problem.

# Solution

- Our main results show that we can do much better by replacing the Schnorr signature scheme in [35] by BLS signatures [13].
  - The resulting schemes are an extremely good fit for Bitcoin, but are also very useful wherever multi-signatures are needed.
- In Section 3 we propose a different defense against the rogue public-key attack that retains all the benefits of both defenses above without the drawbacks.
- In Section 5, we present a modification of the scheme by Maxwell et al. that we prove secure under the standard discrete-log assumption.

# Methodology

## Preliminaries

### Bilinear Group

### Computational Problems

- Discrete Log Problem
- Computational co-Diffie-Hellman Problem
- Computational $\psi$-co-Diffie-Hellman Problem

### Multi-Signatures and Aggregate Multi-Signatures

#### Multi-Signatures

- $Pg$
  - A TTP generates the system parameters
  - $par \leftarrow Pg$
- $Kg$
  - Every signer geenrates a key pair
  - $(pk, sk) \stackrel{s}{\leftarrow} Kg(par)$
- $Sign$
  - Signers collectively sign a message $m$
  - $Sign(par, \mathcal{PK}, sk, m)$
  - where $\mathcal{PK} $ is the set of public keys of the signers
  - At the end, each signer outputs a signature $\sigma$
- $KAg$
  - $KAg(\mathcal{PK}) \rightarrow apk$
  - Input a set of public keys $\mathcal{PK}$ and outputs a single aggregate public key $apk$
- $Vf$
  - Check the validity of a signature
  - $Vf(par, apk, m, \sigma) \rightarrow \{0, 1\}$

#### Aggregate Multi-Signatures

Allows multiple multi-sigs to be aggregated into one.

MultiSig + the following algorithm:
- $SAg$
  - Takes input a ser of tuples $(apk, m, \sigma)$
  - Output a single aggregate multisignature $\sum$

# Relevance

- MultiSig and Aggregate MultiSig is critical for wallet security and blockchain size

# To discuss/investigate

- Relationship between BIP32/BIP44?


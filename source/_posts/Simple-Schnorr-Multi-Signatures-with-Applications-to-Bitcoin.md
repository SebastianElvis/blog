---
title: Simple Schnorr Multi-Signatures with Applications to Bitcoin
date: 2019-01-04 19:52:47
categories:
- paper reading
tags:
- MultiSig
- Schnorr
---



# Info

- **Title**: Simple Schnorr Multi-Signatures with Applications to Bitcoin
- **Conference**: No
- **Year**: 2018
- **URL**: https://eprint.iacr.org/2018/068.pdf
- **Source code**: No

# Addressed Problems

- Multi-signature protocols allow a group of signers (each possessing its own private/public key pair) to produce a single signature $\sigma$ on a message $m$.
- However, the size of the multi-signature in that case grows linearly with the number of signers.
- A serious concern when dealing with multi-signature schemes are **rogue-key attacks**
  - where a subset of $t$ corrupted signers, 1 ≤ t < n, use public keys $pk'_{n−t+1}, \dots , pk'_{n}$ computed as functions of public keys of honest users $pk_{1}, \dots , pk_{n−t}$, allowing them to easily produce forgeries for the set of public keys $\{pk_{1}, \dots , pk_{n−t}, pk'_{n−t+1}, \dots , pk'_{n}\}$ (even though they may not know the secret keys associated with $pk'_{n−t+1}, . \dots , pk'_{n}$).
- One way to generically prevent rogue-key attacks is to require that users prove knowledge (or possession [RY07]) of the secret key during public key registration with a certification authority, a setting known as the **knowledge of secret key (KOSK) assumption**.
- To date, the most practical multi-signature scheme provably secure without any assumption on the key setup has been proposed by **Bellare and Neven (BN)** [BN06] and is based on the **Schnorr signature scheme** [Sch91].

# Solution

- We propose a new Schnorr-based multi-signature scheme which can be seen as a variant of the BN scheme allowing key aggregation in the plain public-key model.

# Methodology

## Original Bellare-Neven MultiSig

Private keys: $x_{1}, \dots , x_{n}$
Public keys: $X_{i} = x_{i}G$

1. $L = H(X_{1}, \dots , X_{n})$
2. Each signer chooses a random nonce $r_{i}$, and shares $R_{i} = r_{i}G$ with other signers
3. $R = \sum R_{i}$
4. Each signer computes $s_{i} = r_{i} + H(L, X_{i}, R, m)x_{i}$, and $s = \sum s_{i}$
5. The final signature $\sigma = (R, s)$ 
6. Verification requires $sG \stackrel{?}{=} R + \sum H(L, X_{i}, R, m)X_{i}$

## Our approach MuSig

MuSig enables key aggregation property without losing security

1. $L = H(X_{1}, \dots , X_{n})$, $X = \sum H(L, X_{i})X_{i}$
2. Each signer chooses a random nonce $r_{i}$, and shares $R_{i} = r_{i}G$ with other signers
3. $R = \sum R_{i}$
4. Each signer computes $s_{i} = r_{i} + H(X, R, m)H(L, X_{i})x_{i}$, $s = \sum s_{i}$
5. The final signature $\sigma = (R, s)$
6. Verification requires $sG \stackrel{?}{=} R + H(X, R, m)X$

## Key aggregation

Key aggregation means you can combine multiple public keys and simplify the verification process.

- BN does not support key aggregation
- MuSig does

## Interactive Aggregate Signatures



# Relevance

- MultiSig is critical for wallet security

# To discuss/investigate

- Implement this scheme?
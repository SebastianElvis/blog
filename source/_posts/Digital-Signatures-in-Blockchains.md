---
title: Digital Signatures in Blockchains
date: 2018-12-25 12:49:08
categories:
- blockchain
- digital signatures
tags:
- ECDSA
- Schnorr
- BLS
- MultiSig
---

# Evolution of Digital Signatures for Blockchain

1. ECDSA based on Secp256k1
2. Segwit
3. MultiSig
4. Schnorr Signatures
5. BLS

https://medium.com/cryptoadvance/how-schnorr-signatures-may-improve-bitcoin-91655bcb4744
https://medium.com/cryptoadvance/bls-signatures-better-than-schnorr-5a7fe30ea716

# ECDSA

Choose an Elliptic Curve with base point $G$ and order $n$ ($nG = O$)

- **GEN**:
  - private key $p$
  - public key $P = pG$
- **SIGN**:
  - message $m$, $z = H(m)$
  - random number $k \in [1, n-1]$
  - $R = (r, \_) = kG$
  - $s = k^{-1}(z + rp)$
  - signature $\sigma = (r, s)$
- **VERIFY**:
  - check $sr \stackrel{?}{=} zG + rP$

![](ecdsa.png)

# SegWit

Segregated Witness:
- The signature data called *witness* will be seperated from the Merkle Tree record.

Why Segwit:
- Block size limit
- Scalability and malleabilituy



# MultiSig

- https://cseweb.ucsd.edu/~mihir/papers/multisignatures-ccs.pdf
- https://eprint.iacr.org/2018/483.pdf



# Schnorr

- **GEN**:
  - private key $p$
  - public key $P = pG$
- **SIGN**:
  - message $m$
  - random number $k \in [1, n-1]$
  - random point $R = kG$
  - $s = k + H(P, R, m) \cdot p$
  - signature $\sigma = (R, s)$
- **VERIFY**:
  - check $s \cdot G = R + H(P, R, m) \cdot P$

![](schnorr.png)

## Applications

### Batch Validation

- With Schnorr we can add up all signature verification equations and verify only once
- $(s_{1} + \dots + s_{1000})G = (R_{1}+ \dots +R_{1000}) + (H(P_{1},R_{1},m_{1})P_{1} + \dots + H(P_{1000},R_{1000},m_{1000})P_{1000})$

![](batch-validation.png)

###Key Aggregation

- Generate a shared public key with a pair of private keys
- $P = P_{1} + P_{2} = p_{1}G + p_{2}G$
- ...
- Signature $\sigma = (R_{1}+R_{2}, s_{1}+s_{2})$

### MuSig

- https://blockstream.com/2018/01/23/musig-key-aggregation-schnorr-signatures.html

# BLS

https://medium.com/cryptoadvance/bls-signatures-better-than-schnorr-5a7fe30ea716
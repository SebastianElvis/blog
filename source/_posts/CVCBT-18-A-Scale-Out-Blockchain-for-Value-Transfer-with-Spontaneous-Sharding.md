---
title: CVCBT'18 - A Scale-Out Blockchain for Value Transfer with Spontaneous Sharding
date: 2019-01-03 20:41:00
tags:
---

# Info

- **Title**: A Scale-out Blockchain for Value Transfer with Spontaneous Sharding
- **Conference**: Crypto Valley Conference on Blockchain Technology (CVCBT)
- **Year**: 2018
- **URL**: https://arxiv.org/abs/1801.02531
- **Source code**: https://github.com/blockchain-lab/ScaleOutDistributedLedger

# Addressed Problems

- Recently, many blockchains have been proposed to achieve scale-out throughput by letting nodes only acquire a fraction of the whole transaction set.
- However, these schemes, e.g., sharding and off-chain techniques, suffer from a degradation in decentralization or the capacity of fault tolerance.

# Solution

- We formally define the Value Transfer Ledger (VTL) and distinguish it from other types of ledgers and databases
- We propose an off-chain based blockchain system that prevents double-spending without sacrificing either security or decentralization. In particular, our consensus algorithm achieves uncompromised agreement and validity conditions of the BFT consensus in VTL model.
- We prove that our system is a valid VTL system. In other words, although our system do not guarantee BFT for generic types of data, we guarantee that if all nodes have interest in their values in the system and behave rationally, the valid transactions in our system are double- spending-proof.
- It is shown that the CCPT of our system is upper bounded by O(N), which suggests scalable throughput. Moreover, we show that our system could achieve scale-out through- put via spontaneous sharding in several scenarios.

# Methodology

## Model

- Every node holding some initial value
- Transactions are defined as UTXO. We assume a transaction has only one sender and one receiver.
- We consider a weak asynchronous network with $f \leq \frac{N−1}{3}$ Byzantine adversaries
- Unbreakable hash function $Y = H(X)$ and a digital signature scheme $Y = Sig_{i}(X)$



# Relevance

- Sharding is a promising solution to scale blockchains

# To discuss/investigate

- This proposal seems not to include the cross-sharding transactions.

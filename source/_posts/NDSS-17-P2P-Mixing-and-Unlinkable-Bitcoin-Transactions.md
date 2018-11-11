---
title: NDSS'17 - P2P Mixing and Unlinkable Bitcoin Transactions
date: 2018-11-11 11:56:28
categories:
- paper reading
tags:
- NDSS
- Blockchain
- Privacy
- P2P Mixing
- Unlinkable Transactions
---


# Info

- **Title**: P2P Mixing and Unlinkable Bitcoin Transactions
- **Conference**: USENIX NDSS
- **Year**: 2017
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2016/824.pdf
- **Source code**: No

# Background: Dining Cryptographers Network (DC-nets)

## Background

- The **Dining Cryptographers Problem** studies how to perform a secure multi-party computation of the boolean-OR function. 
- David Chaum first proposed this problem in the early 1980s and used it as an illustrative example to show that it was possible to send anonymous messages with unconditional sender and recipient untraceability. 
- Anonymous communication networks based on this problem are often referred to as **DC-nets** (where DC stands for "dining cryptographers").
- The Dining Cryptographers Problem is unrelated to the Dining Philosophers Problem.

## Description

- Three cryptographers gather around a table for dinner. - The waiter informs them that the meal has been paid for by someone, who could be one of the cryptographers or the National Security Agency (NSA).
- The cryptographers respect each other's right to make an anonymous payment, but want to find out whether the NSA paid. 
- So they decide to execute a two-stage protocol.

## Solution of David Chaum

1. Each cryptographer flips an unbiased coin (in secret)
2. Each shows the result to the person on the right
3. Each cryptographer states whether the two coins he can see are the same or different
    - If one of the cryptographers is the payer he says the opposite of what he sees

- An odd number of differences means that a cryptographer has paid 
- Otherwise the NSA paid

**The algorithm is extensible to any number of diners**



# Addressed Problems

1. The original DC-net protocol is prone to disruption by a single malicious peer who sends invalid protocol messages entirely.
2. The original DC-net protocol protects the anonymity of the involved malicious peers, making it impossible for honest peers to detect and exclude the malicious peer.
3. Successors of the DC-net protocols are still not ideal: with communication rounds quadratic in the worst case with many malicious peers, these current pure P2P solutions [22], [52], [55] **do not scale** as the number of participating peers grows.

# Solution

In this paper, it is our goal to bring P2P anonymous communication from the realm of feasibility to the realm of **practicality**.

- We conceptualize P2P mixing as a natural generalization of DC-nets [17].
- We present the new P2P mixing protocol *DiceMix*, which builds on the original DC-net protocol. *DiceMix* requires only **4+2f** rounds (worst) in the presence of **f** malicious peers, i.e., only four rounds if every peer behaves honestly.
- We apply *DiceMix* to Bitcoin. Building on the *CoinJoin* paradigm [43] and *DiceMix*, we present *CoinShuffle++*, a practical decentralized mixing protocol for Bitcoin users. 
    - *CoinShuffle++* considerably simpler and thus easier to implement than its predecessor CoinShuffle [52]
    - *CoinShuffle++* inherits the efficiency of DiceMix and thus outperforms CoinShuffle significantly.
- We present a deanonymization attack on existing P2P mixing protocols that guarantee termination in the presence of disruptive peers.
    - We discuss how *DiceMix* resists this attack by requiring fresh input messages (e.g., cryptographic keys never used before), and we discuss why this is not a problem for applications such as coin mixing.


# Methodology



# Relevance

- Confidential transaction is one of our research topics.
- The underlying DC-nets protocol is highly related to the consensus problem. 


# To discuss/investigate

- It should be interesting to implement an open-source version and evaluate it.
- We can utilize this technique to other applications, like distributed storage and outsourced computing.
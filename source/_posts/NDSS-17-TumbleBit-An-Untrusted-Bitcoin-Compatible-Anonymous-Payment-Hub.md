---
title: 'NDSS''17 - TumbleBit: An Untrusted Bitcoin-Compatible Anonymous Payment Hub'
date: 2018-11-18 13:54:35
categories:
- paper reading
tags:
- Blockchain
- Confidential Transactions
- NDSS
---



# Info

- **Title**: TumbleBit: An Untrusted Bitcoin-Compatible Anonymous Payment Hub
- **Conference**: USENIX NDSS
- **Year**: 2017
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2016/575.pdf
- **Source code**: https://github.com/BUSEC/TumbleBit/

# Addressed Problems

- The sheen of Bitcoin anonymity has all but worn off, dulled by a stream of academic papers [38], [51], and a blockchain surveillance industry [32], [26], that have demonstrated weak-nesses in Bitcoin’s anonymity properties.

# Solution

- We present TumbleBit, a *unidirectional unlinkable payment hub* that uses an untrusted intermediary, the Tumbler *T*, to enhance anonymity.
    - TumbleBit allows a payer A to send fast off-blockchain payments (of denomination one bitcoin) to a set of payees (B1, ..., BQ) of her choice.
    - Off-chain, so scalable, high performance and low latency
- We implemented TumbleBit as a classic tumbler. (That is, each payer and payee can send/receive Q = 1 payment/epoch.) We then used TumbleBit to mix bitcoins from 800 payers (Alice A) to 800 payees (Bob B).

# Methodology

- TumbleBit replaces on-blockchain payments with **off-blockchain puzzle solving**
    - where Alice $A$ pays Bob $B$ by providing $B$ with the solution to a puzzle. 
    - The puzzle $z$ is generated through interaction between $B$ and $T$, and solved through an interaction between $A$ and $T$. 
    - Each time a puzzle is solved, 1 bitcoin is transferred from Alice $A$ to the Tumbler $T$ and finally on to Bob $B$.
- The protocol proceeds in three phases
    - Escrow Phase
        - Alice $A$ opens a payment channel with the Tumbler $T$ by escrowing $Q$ bitcoins on the blockchain
        - Payee Bob $B4 also opens a channel with $T$
            - $T$ escrowing $Q$ bitcoins on the blockchain
            - $B$ and $T$ engaging in a puzzle-promise protocol that generates up to $Q$ puzzles for $B$.
    - Payment Phase
        - $A$ makes up to Q off-blockchain payments to any set of payees
        - To make a payment, $A$ interacts with $T$ to learn the solution to a puzzle $B$ provided
    - Cash-Out Phase
        - closes all payment channels
        - $B$ uses his $Q'$ solved puzzles (aka, TumbleBit payments) to create an on-blockchain transaction that claims $Q'$ bitcoins from $T$’s escrow
        - $A$ also closes her escrow with $T$, recovering bitcoins not used in a payment

# Relevance

- This is a confidential transaction scheme compatible with Bitcoin, but requires a trusted third party
- The RSA puzzle based puzzle-promise protocol is an efficient method of mixing TXs 

# To discuss/investigate

- Is there any related work about the RSA puzzle promise protocol?
- Here is a family of confidential transaction schemes compatible with Bitcoin. Should we summary them?
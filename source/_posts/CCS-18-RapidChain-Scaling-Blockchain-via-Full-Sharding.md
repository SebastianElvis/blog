---
title: 'CCS''18 - RapidChain: Scaling Blockchain via Full Sharding'
date: 2018-11-17 14:26:46
categories:
- paper reading
tags:
- Blockchain
- Sharding
- Scalability
- CCS
---


# Info

- **Title**: RapidChain: Scaling Blockchain via Full Sharding
- **Conference**: ACM CCS
- **Year**: 2018
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2018/460.pdf
- **Source code**: No

# Addressed Problems

- Existing solutions currently either fail to solve the above challenges or make security/performance trade-offs that, unfortunately, make them no longer truly-decentralized solutions.
    - In particular, traditional Byzantine consensus mechanisms such as [17, 19, 43] can only work in a **closed membership** setting, where the set of participants is fixed and their identities are known to everyone via a **trusted third party**.
    - If used in an open setting, these protocols can be easily compromised using Sybil attacks [25], where the adversary repeatedly rejoins malicious parties with fresh identities to gain significant influence on the protocol outcome.
- The trade-off between the security (adversary model) and the performance is hard
    - Most traditional schemes assume a static adversary who can select the set of corrupt parties only at the start of the protocol. 
    - Existing protocols that are secure against an adaptive adversary such as [12, 18, 37] either scale poorly with the number of participants or are inefficient.
- While most of these protocols can reportedly improve the throughput and latency ofBitcoin, all ofthem still require the often-overlooked assumption of **a trusted setup** to generate an unpredictable initial common randomness in the form of a common genesis block to bootstrap the blockchain.
- In addition to being partially decentralized, most of these solutions have either **large per-node storage requirements** [4, 23, 28, 40, 46, 51, 54], **low fault resiliency** [41, 46], **incomplete specifications** [23, 40], or other s**ecurity issues** [40, 41, 46]


# Solution

- We propose RapidChain, a Byzantine-resilient public blockchain protocol that improves upon the scalability and security limitations
    - RapidChain **partitions** the set of nodes into multiple smaller groups of nodes called *committees* that operate **in parallel** on **disjoint** blocks of transactions and maintain **disjoint** ledgers.
    - Such a partitioning of operations and/or data among multiple groups of nodes is often referred to as *sharding* [21] and has been recently studied in the context of blockchain protocols [41, 46].
    - Properties:
        - Sublinear Communication: $O(n)$ bits exchanged per TX
        - Higher Resiliency: $\frac{1}{3}$ malicious nodes
        - Rapid Committee Consensus: 3-10 times faster compared to previous solutions
        - Secure Reconfiguration: the Cuckoo rule [8, 60] to provably protect against a slowly-adaptive Byzantine adversary
        - Fast Cross-Shard Verification
            - each node stores only a $\frac{1}{k}$ fraction of the entire blockchain with $k$ committees
            - Kademlia routing -> $\log(n)$ latency and storage
        - Decentralized Bootstrapping: bootstrap **with** only $O(n\sqrt{n})$ messages **without** common randomness (genesis block)
- We also implement a prototype of RapidChain to evaluate its performance and compare it with the state-of-the-art sharding-based protocols.
  

# Methodology

## Network Model

- Sybil attack-resistant: by PoW variants
- Synchronous communication only during our intra-committee consensus
- Partially-synchronous channels [19] between nodes with exponentially-increasing time-outs (similar to [41]) to minimize latency and achieve responsiveness.

## Threat Model

- Nodes may disconnect from the network during an epoch or between two epochs **due to any reason** such as internal failure or network jitter
- PPT Byzantine adversary who corrupts $t \lt \frac{n}{3}$ of the nodes at any time
- the adversary is *slowly adaptive*, that it is allowed to select the set of corrupt nodes at the beginning of the protocol and/or between each epoch but **cannot change this set within the epoch**
- At the end of each epoch, the adversary is allowed to corrupt a constant (and small) number of uncorrupted nodes while maintaining their identities
- the adversary can run a join-leave attack [8, 25]
  
## Lifecycle

- Bootstrapping
- Consensus
    - Each ommittee accpets TXs independently
        - Partially synchronous
        - Inter-committee routing to find which committee the client should send TXs to
    - Intra-committee Consensus
        - Synchronous consensus in fixed rounds
        - Info Dispersal Algorithm (IDA) based gossip protocol
- Reconfiguration
    - For renewing committees
    - PoW only for combatting Sybil attacks
    - Epoch randomness generation by *Distributed Randomness Generation* (DRG)
    - Cuckoo replacement policy

# Relevance

- Sharding is one of the most potential approaches to scale public blockchains
- This research reviews state-of-the-art work in detail
- This research covers different aspects (layers) of Blockchain (consensus, network, routing, ...)

# To discuss/investigate

- Could we reproduce the performance evaluation experiments?
- It is necessary to develop an open-sourced sharding framework, as those committee-based protocols are quite similar and modularized
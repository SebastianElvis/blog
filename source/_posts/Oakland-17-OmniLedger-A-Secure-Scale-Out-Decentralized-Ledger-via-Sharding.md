---
title: >-
  Oakland'17 - OmniLedger: A Secure, Scale-Out, Decentralized Ledger via
  Sharding
date: 2018-12-05 18:17:40
categories:
- paper reading
tags:
- Sharding
- Blockchain
- Scalability
---


# Info

- **Title**: OmniLedger: A Secure, Scale-Out, Decentralized Ledger via Sharding
- **Conference**: IEEE S&P
- **Year**: 2017
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2017/406.pdf
- **Source code**: https://github.com/dedis/student_17_bftcosi

# Addressed Problems

- Existing proposals for sharded DLs forfeit permissionless decentralization [16], introduce new security assumptions, and/or trade performance for security [34]
  - ByzCoin: It's total processing capacity does not increase with participation i.e., it does not scale-out.
  - RandHound [44] is a scalable, SMPC protocol that provides unbiasable, decentralized randomness in a Byzantine setting.
  - Elastico: 
    - Elastico's relatively small shards (e.g., 100 validators per shard in experiments) yield a high failure- probability of 2.76% per shard per block under a 25% adversary, which cannot safely be relaxed in a PoW system [23].
    - Elastico’s shard selection is not strongly bias-resistant, as miners can selectively discard PoWs to bias results [7].
    - Elastico does **not** ensure transaction atomicity across shards, leaving funds in one shard locked forever if another shard rejects the transaction.
    - the validators constantly switch shards, forcing themselves to store the global state, which can hinder performance but provides stronger guarantees against adaptive adversaries.
    - the latency of transaction commitment is comparable to Bitcoin (≈ 10 min.), which is far from OmniLedger’s usability goals.

# Solution

- We introduce the first DL architecture that provides horizontal scaling without compromising either long-term security or permissionless decentralization.
- We introduce Atomix, a Atomic Commit protocol, to commit transactions atomically across shards.
- We introduce ByzCoinX, a BFT consensus protocol that increases performance and robustness to DoS attacks.
- We introduce state blocks, that are deployed along OmniLedger to minimize storage and update overhead.
- We introduce two-tier trust-but-verify processing to minimize the latency of low-value transactions.

# Methodology

## System Overview

### System Model

- $n$ validators who process TXs and ensure the consistency
- Each validator $i$ has a public/private key pair $(pk_{i}, sk_{i})$, and we often identify $i$ by $pk_{i}$.
- We assume that the configuration parameters of a shard $j$ are summarized in a *shard-policy file*
- We denote by an epoch $e$ the fixed time (e.g., a day) between global reconfiguration events 
  - where a new assignment of validators to shards is computed.
  - The time during an epoch is counted in rounds $r$ that **do not** have to be consistent between different shards.
- During each round $r$, each shard processes TXs collected from clients.
- We assume that validators can establish identities through any Sybil-attack-resistant mechanism and commit them to the identity blockchain.
- To participate in epoch $e$ validators have to register in epoch $e−1$
- These identities are added into an identity blockchain
  
### Network Model

- The network graph of honest validators is well connected
- The communication channels between honest validators are synchronous
  - if an honest validator broadcasts a message, then all honest validators receive the message within a known maximum delay $\delta$ [39].
  - However, as $\delta$ is in the scale of **minutes**, we cannot use it within epochs as we target latencies of seconds.

In conclusion, all protocols inside one epoch use the **partially synchronous model** [13] with optimistic, exponentially increasing time-outs, whereas $\delta$ is used for slow operations such as identity creation and shard assignment.

### Threat Model

- We denote the number of Byzantine validators by $f$ 
- We assume that $n = 4f$ 
  - i.e., **at most 25%** of the validators can be malicious at any given moment
  - which is similar to prior DL’s [21], [32], [34].
- These malicious nodes can behave arbitrarily
  - e.g., They might refuse to participate or collude to attack the system.
- The remaining validators are honest and faithfully follow the protocol.
- We further assume that the adversary is *mildly adaptive* [31], [34] on the order of epochs, 
  - i.e., He can try to corrupt validators, but it takes some time for such corruption attempts to actually take effect.
- We further assume that the adversary is **computationally bounded**, that cryptographic primitives are secure, and that the computational Diffie-Hellman (CDH) problem is hard.
  
### System Goals

- Full decentralization
  - no single point of failure or third parties
- Shard robustness
  - Each shard correctly and continuously processes transactions assigned to it.
- Secure TXs
  - Transactions are committed atomically or eventually aborted, both within and across shards.
- Scale-out
  - The expected throughput of OmniLedger increases linearly in the number of participating validators.
- Low storage overhead
  - Validators do not need to store the full transaction history but only a periodically computed reference point that summarizes a shard’s state.
- Low latency
  - OmniLedger provides low latency for transaction confirmations

### Architecture Overview

1. At the beginning of an epoch $e$, all validators run **RandHound** to re-assign randomly a certain threshold of validators to new shards and assign new validators who registered to the identity blockchain in epoch $e−1$.
2. Validators ensure consistency of the shards’ ledgers via **ByzCoinX**
3. Clients ensure consistency of their cross-shard transactions via **Atomix** 
  - Here the client spends inputs from shards 1 and 2 and outputs to shard 3.

- Distributed Randomness Generation by **RandHound + VRF**
- Atomically processing transactions across shards by **Atomix** 
  - such that each transaction is either committed or eventually aborted.
  - The purpose is to ensure consistency of transactions between shards, to prevent double spending and to prevent unspent funds from being locked forever.
  1. Initialize
  2. Lock
  3. Unlock
    - Unlock to Commit
    - Unlock to Abort
- Fault Tolerance under Byzantine Faults via **ByzCoinX**


# Relevance

- Sharding is strongly related with our research topic: scalability
- RandHound is based on MPC, which is a strong crypto primitive

# To discuss/investigate

- Maybe we can benchmark the existing implementation?
- Does RandHound have implementations?
- Cryptographic sortition seems like an interesting topic

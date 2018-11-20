---
title: 'USENIX Security''17 - SmartPool: Practical Decentralized Pooled Mining'
date: 2018-11-17 17:32:17
categories:
- paper reading
tags:
- Blockchain
- Mining Pool
- USENIX Security
---


# Info

- **Title**: SmartPool: Practical Decentralized Pooled Mining
- **Conference**: USENIX Security
- **Year**: 2017
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2017/019.pdf
- **Source code**: https://github.com/SmartPool

# Addressed Problems

- Centralized pool operators direct the massive computational power of their pools’ participants
    - Bitcoin derives at least 95% of its mining power from only 10 mining pools
    - Ethereum network has 80% of its mining power emanating from 6 pools
- Recent work by Apostolaki et al. has demonstrated large-scale network attacks on cryptocurrencies
    - e.g. double spending and network partitioning
- Pools currently dictate which transactions get included in the blockchain, thus increasing the threat of transaction censorship significantly
- Existing decentralized mining pools are not widely applied, as technical challenges have hindered widespread adoption.
    - Scalable participation under P2POOL’s current design would require the system to check a massive number of sub-puzzles
    - P2POOL only works for Bitcoin; we are not aware of any decentralized mining approach for Ethereum.

# Challenges

- Decentralization
- Efficiency
- Security
- Fairness

# Solution

- we observe that it is possible to run a decentralized pool mining protocol as a smart contract on the Ethereum cryptocurrency
    - Our solution layers its security on the existing mining network of a large and widely deployed cryp-tocurrency network, thereby mitigating the difficulty of bootstrapping a new mining network from scratch.
- we propose a design that is efficient and scales to a large number of participants.
    - Our design uses a simple yet powerful probabilistic verification technique which guarantees the fairness of the payoff.
    - We also introduce a new data structure, the augmented Merkle tree, for secure and efficient verification.
    - SMARTPOOL allows miners to freely select which transaction set they want to include in a block.
    - SMARTPOOL does not charge any fees, unlike centralized pools, and disburses all block rewards to pool participants entirely.
- We demonstrate the standard pay-per-share (or PPS) scheme in our implementation. 
    - Supporting other standard schemes like pay-per-last-n-shares (PPLNS) and schemes that disincentivize against block withholding attacks [9–11] is left for future work.
- We implemented SMARTPOOL and deployed real mining pools on **Ethereum and Ethereum Classic**. The pools have so far mined 105 real blocks and have handled significant hashrates while deferring only 0.6% of block rewards to transaction fee costs.

# Methodology

- SMARTPOOL is a smart contract which implements a decentralized mining pool for Ethereum and runs on the Ethereum network.
    - SMARTPOOL maintains two main lists in its contract state
        - *claimList*
            - When a miner submits a set of shares as claim for the current Ethereum block, it is added to the claimList
            - Each claim allows miners to submit a batch of (say, 1 million) shares.
            - acts as a cryptographic commitment to the set of shares claimed to be found by the miner
            - Each claim specifies **the number of shares** the miner claims to have found, and it has a particular structure that aids verification in a subsequent step
        - *verClaimList*
            - SMARTPOOL then proceeds to verify the validity of the claim
            - once verified, it moves it to the *verClaimList*
            - Claim verification and payments for verified claims happen **atomically** in a **single Ethereum transaction**.
    - Chaim submissions
        - SMARTPOOL defines a *Claim* structure which consists of a few pieces of data
        - The miner cryptographically commits to the set of shares he is claiming.
            - The cryptographic commitment goes via a specific data structure we call an **augmented Merkle tree**, as discussed in Section 3.5.
        - **Probabilistic verification**
            - Searching for existing shares
            - Checking validity of shares
    - Augmented Merkle Tree
        - Each share is mapped to an integer
        - Each node has the form {$mix(x)$, $hash(x)$, $max(x)$}
        - $mix(x)$/$max(x)$ is the minimum/maximum of x's children
        - An augmented Merkle tree is called *sorted* if all of its leaves occur in strictly increasing order from left to right with respect to its counter function
        - *Batch submission* with *Augmented Merkle Tree*

# Relevance

- I'm a core developer of [MATPool](matpool.io), which is the mining pool for [Bytom](bytom.io)
- Decentralize the mining pool is critical for the cryptocurrency ecosystem, in order to circumvent the censorship

# To discuss/investigate

- I don't think overlaying the mining pool upon the Ethereum smart contracts can achieve acceptable efficiency
    - Mining pool's reward highly relies on the network latency, and the smart contract's efficiency will make the mining reward dropping radically
- What if we keep using the centralized mining pool, but leave the disputes to smart contracts?
- It is not necessary to decentralize the mining pool by the smart contracts, but we can utilize existing P2P systems like Kademlia
- What if we develop a hierarchical mining pool which can overlay each other?
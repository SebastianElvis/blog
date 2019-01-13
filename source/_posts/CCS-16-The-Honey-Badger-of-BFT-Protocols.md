---
title: CCS'16 - The Honey Badger of BFT Protocols
date: 2019-01-13 15:37:33
categories:
- paper reading
tags:
- Scalability
- Consensus
---


# Info

- **Title**: The Honey Badger of BFT Protocols
- **Conference**: CCS
- **Year**: 2016
- **URL**: https://eprint.iacr.org/2016/199
- **Source code**: https://github.com/amiller/HoneyBadgerBFT

# Addressed Problems

- Protocols relying critically on network timing assumptions and only guarantee liveness when the network behaves as expected are ill-suited for public Blockchain scenario.

# Solution

- Positions
  - Robustness is a first-class citizen.
  - Favor throughput over latency
  - Timing assumptions considered harmful
- We propose HoneyBadgerBFT, the first BFT atomic broadcast protocol to provide optimal asymptotic efficiency in the asynchronous setting
  - Communication complexity: Best $O(n)$, Worst: $O(n)$
    - state-of-the-art: both $O(n^{2})$
  - HoneyBadgerBFT’s design is optimized for a cryptocurrency-like deployment scenario 
    - where network bandwidth is the scarce resource, but computation is relatively ample. 
  - This allows us to take advantage of cryptographic building blocks (in particular, threshold public-key encryption) 
    - that would be considered too expensive in a classical fault-tolerant database setting where the primary goal is to minimize response time even under contention

# Methodology

TODO

# Relevance

- HoneyBadgerBFT is the first practical BFT-style consensus protocol for public blockchains 

# To discuss/investigate

- Evaluation?
- How to make it not require fixed participants?
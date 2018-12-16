---
title: P2P'13 - Information Propagation in the Bitcoin Network
date: 2018-12-16 14:48:07
categories:
- paper reading
tags:
- Bitcoin
- P2P Network
---


# Info

- **Title**: Information Propagation in the Bitcoin Network
- **Conference**: IEEE P2P
- **Year**: 2013
- **URL**: https://www.tik.ee.ethz.ch/file/0bc1493ba049fe69dbafccef4220c666/presentation.pdf
- **Source code**: No

# Addressed Problems

- Any inconsistency introduces uncertainty about the validity of a given transaction.
- An inconsistency may jeopardize the security of the consensus itself.


# Solution

- In this work we analyze Bitcoin from a networking perspective
  - e.g. How information is disseminated or propagated in the Bitcoin network?
    - We identify key weaknesses as well as the resulting problems.
  - We analyze the synchronization mechanism which fails to synchronize the information stored at the ledger with a non-negligible probability.
    - This problem not only causes a prolonged inconsistency that goes unnoticed by a large number of nodes, but also weakens the system's defenses against attackers.
    - We then propose some changes to the current protocol that, while not a solution to the intrinsic problems of the communication model used by Bitcoin, may mitigate them.

# Methodology

## Info Propagation Method

- Only transaction (tx) and block (block) messages are relevant to the info propagation
- *inv* message
  - invite a node to ask for data from itself
  - contains a set of transaction hashes and block hashes that have been received by the sender and are now available to be requested
- *getdata* message
  - select data it wants
  - after receiving *inv*, respond a *getdata* message
  - containing the hashes of the information it needs
- *block* message
  - actual data transfer
- At each hop in the broadcast the message incurs in a propagation delay.

## Blockchain Forks

## Speeding Up the Propagation

- Minimize verification 
  - There is a strong correlation between the size of a block and the time to verify it
  - verification can be divided into two phases:
    - An initial difficulty check
      - if meet difficulty
      - if duplicated
    - A transaction validation
  - Therefore could be changed to send an *inv* message as soon as the difficulty check is done, instead of waiting for the considerably longer transaction validation to be finished.
- Pipelining block propagation
  - immediately forwarding incoming *inv* messages to neighbors
  - amortize the round-trip times between the node and its neighbors
- Connectivity increase

# Relevance

- P2P network synchronization lacks measurement and optimization

# To discuss/investigate

- What else P2P network measurement we can do?
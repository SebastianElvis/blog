---
title: Preprint - Scaling Nakamoto Consensus to Thousands of Transactions per Second
date: 2019-01-13 12:33:36
categories:
- paper reading
tags:
- Scalability
- DAG
---



# Info

- **Title**: Scaling-Nakamoto-Consensus-to-Thousands-of-Transactions-per-Second
- **Conference**: No
- **Year**: 2018
- **URL**: https://arxiv.org/abs/1805.03870
- **Source code**: No

# Addressed Problems

- However, the performance bottleneck remains one of the most critical challenges of current blockchains.
- In the standard Nakamoto consensus, the performance is bot- tlenecked by the facts
  - only one participant can win the competition and contribute to the blockchain, i.e., concurrent blocks are discarded as forks
  - the slowness is essential to defend against adversaries
- Hours of waiting before confirming a transaction.

# Solution

- We present Conflux, a fast, scalable, and decentralized blockchain system that can process thousands of trans- actions per second while confirming each transaction in minutes.
- By defering the transaction total ordering and optimistically process concurrent transactions and blocks.
- Given that the transactions rarely conflict in blockchains (particularly in cryptocurrencies), Conflux first optimistically assumes that transactions in concurrent blocks would not conflict with each other by default.

# Methodology

## The Conflux DAG

### DAG 
(e.g. $A \leftarrow B$ where A and B are blocks, and A is older than B)
- Parent Edge: The edge is the Parent Edge for B.
- Reference Edge: The edge is the Reference Edge for A.

### Pivot Chain:

- all parent edges in a DAG together form a parental tree in which the genesis block is the root.
- Conflux selects a chain from the genesis block to one of the leaf blocks as the pivot chain.
- Run the GHOST rule (same as Ethereum)

### Generating New Block

- It first computes the pivot chain in its local DAG state and sets the last block in the chain as the parent of the new block.

### Epoch

- Parent edges, reference edges, and the pivot chain together enable Conflux to split all blocks in a DAG into epochs.
- Every block in the pivot chain corresponds to one epoch
- Each epoch contains all blocks
  - that are reachable from the corresponding block in the pivot chain via the combination of parent edges and reference edges
  - that are not included in previous epochs
  
### Block Total Order

- Conflux first sorts the blocks based on their corresponding epochs and then sorts the blocks in each epoch based on their topological order.
- If two blocks in an epoch have no partial order relationship, Conflux breaks ties deterministically with the unique ids of the two blocks.

### Transaction Order

- Conflux first sorts transactions based on the total orders of their enclosing blocks. 
- If two transactions belong to the same block, Conflux sorts the two transactions based on the appearance order in the block.

## Conflux Consensus - The First DAG-based Nakamoto Consensus

- Use whatever consensus protocol you want
- But appending the blocks follows the GHOST protocol and the block/TX ordering mechanism above, so blocks/TXs can be concurrent within one epoch
- The consensus consists of 2 events:
  - Receive a new block
    - Verify
    - Update the DAG
    - Broadcast
  - Generate a new block
    - Update the DAG
    - Broadcast

# Relevance

- DAG is a possible solution for concurrent Nakamoto Consensus
- But this cannot shorten the confirmation time

# To discuss/investigate

- Implement this using Ethereum?

---
title: Preprint - A Web of Blocks
date: 2019-01-12 13:52:38
categories:
- paper reading
tags:
- Authenticated Data Structure
- Scalability
---



# Info

- **Title**: A Web of Blocks
- **Conference**: No
- **Year**: 2018
- **URL**: https://arxiv.org/abs/1806.06978
- **Source code**: No

# Addressed Problems

- Blockchain-based applications suffer from a fundamental lack of scalability.
- Each consensus mechanism has some maximum speed, regardless of the number of participating machines [18, 40, 50].
  - As a result, popular chains like Bitcoin [52] and Ethereum are overburdened [29, 37].

# Solution

- We present *Charlote*, a framework for applications built on the blockweb, a novel generalization of blockchains.
  - Whereas blockchains enforce a total ordering on all data, a blockweb requires ordering only when one block references another.
  - This is a natural extension of the database community’s decades old "least ordering" ideal [12].
- Applications can preserve the serializability properties of traditional blockchains while executing multi-chain transactions, using recursive atestations and meets (§ 4).

# Methodology

## Design

- No not force unnecessary ordering
- Storing blocks and choosing blocks in the data structure is two things.
- Incentives left to applications
- Conflicts and reliability should be decided by your own. (DO NOT guarantee the trustworthness and availability)

Two types of servers:
- *Wilbur*: The key-value stores which store blocks
- *Fern*: The integrity servers atest to which blocks belong in which data structures.

## Data Structure

- Each block is a Merkle tree, with 3 leaf types:
  - *Payload*: leaves are application-specific data.
  - *Reference*: leaves identify another block, and provide atestations as to where the block is stored, and what data structures include it (§ 3.1.1).
  - *Label*: leaves describe where this block should be stored, what data structures it must be a part of, and who must approve its membership in those data struc- tures (§ 3.1.2)

- Attestation
  - Each atestation is a promise
  - *Wilbur* servers create availability atestations which promise to store a block.
  - *Fern* servers create integrity atestations which promise not to atest to conflicting blocks.


# Relevance

- This is a distributed system with weaker consistency and data updatability.
- May useful for some applications like Version Control

# To discuss/investigate

- Difference with Git?
- Does normal consensus protocols work properly for this design?
---
title: 'CCS''17 - Revive: Rebalancing Off-Blockchain Payment Networks'
date: 2018-11-17 16:04:49
categories:
- paper reading
tags:
- Blockchain
- Payment Channels
- Smart Contracts
- CCS
---


# Info

- **Title**: Revive: Rebalancing Off-Blockchain Payment Networks
- **Conference**: ACM CCS
- **Year**: 2017
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2017/823.pdf
- **Source code**: https://github.com/rami-khalil/revive

# Addressed Problems

- Refunding a payment channel requires performing transactions on the blockchain, which is expensive
    - Once a payment channel is depleted, the channel needs to be closed and re-funded, requiring at least two expensive on-chain transactions
- Before refunding a channel, users might first opt to choose more expensive channel routes, which will increase the transaction costs over payment channels (each hop in a payment network receives a **relay fee**).


# Solution

- We proposed the first rebalancing scheme REVIVE for off-chain payment networks.
    - enables a set of members in a payment network to shift balances between their payment channels safely
    - our solution allows participants to safely **revive** a channel by re-allocating off-chain funds they have assigned to their payment channels
    - rebalancing with REVIVE is free
    - REVIVE can be applied to different underlying payment networks
- We provide an implementation and evaluation of Revive for the Ethereum network, using the Sprites[7] payment channel.

# Methodology

## System Model

- Blockchain
- Smart Contracts
- Off-chain Payment Network
- Payment Network Topology
- Communication Network
- Adaptability

## Rebalancing Protocol

- Leader Election
    - A leader elected to act as a point of synchronization for the protocol
    - The leader receives enough information about channel balances, calculate a set of rebalancing transactions
    - This leader does not need to be a stakeholder in any of the payment channels that are to be rebalanced, therefore, it may even be a third party chosen by the participants
- Triggering
    - the current leader waits for rebalancing initiation requests from participants in the sub-network
    - the leader sends an initiation request to all participants asking for confirmation of their participation in this round of rebalancing
    - This triggering phase is **customizable** and serves to **set a threshold past** which a rebalancing is considered to be worth executing.
- Participation
    - After receiving the confirmations, the leader announces to the involved participants which nodes are confirmed in the current round, and asks them to freeze the relevant payment channels they wish to rebalance.
- Transaction Set Generation
    - Participants respond which channels they have frozen, along with their respective balances and objectives for the challenge
    - **Mutual agreement** by both owners of a payment channel on the freezing and the balances should be expected. 
    - Participants may submit rebalancing objectives, which specify whether they wish to **gain or lose** credit in each channel. 
    - Mutual agreement by both peers on the direction of rebalance in a channel should also be expected here.
    - The generation is done through **solving a linear program** that produces a set of rebalancing transactions.
- Consensus
    - The transaction set and a list of participating members are then sent **in the form of a commitment** to all nodes for verification and signing. 
    - The commitment is composed of **the merkle-tree root** [20] of all rebalancing transactions, and **a hash of all the participants’ public addresses** (identities), both hashed together.
    - Once all signatures are obtained by the leader, they are multicast to the involved participants.
- Dispute

# Relevance

- Payment Channel can scale the public blockchain greatly without the need of modifying the consensus protocol
- This method seems to be able to greatly reduce the Payment Channel centralization

# To discuss/investigate

- Could we reproduce the experiment with their open-sourced smart contracts?
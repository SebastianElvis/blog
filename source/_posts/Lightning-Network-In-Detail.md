---
title: Lightning Network In Detail
date: 2019-02-25 14:25:30
categories:
- blockchain
- off-chain
tags:
- RSMC
- HTLC
- Off-chain Payment
- Payment Channel
---

# The Lightning Network Protocol

1. Establishing the Duplex Payment Channel
   - By creating a Funding Transaction
2. Making Commitment Transactions
   - Unenforcible TXs
   - tx1, tx2, ... whatever you want
   - 2-2 MultiSig TXs
3. Closing the Duplex Payment Channel

# Establishing the Duplex Payment Channel



# Making Commitment Transactions

## Process

- If Alice and Bob have a payment channel, both of them should have a "latest" **commitment transaction**.
- A **commitment transaction** divides the funds from the funding transaction according to the correct allocation between Alice and Bob
  - e.g. if Alice owns 1.0 mBTC and Bob owns 1.0 mBTC in the channel, the commitment transactions divide the total channel funds in that way

Initially, 

- Alice holds the $A_{1}$ commitment TX, and Bob holds the $B_{1}$ commitment TX
- $R_{A_{1}}$ is the revocation key for $A_{1}$ hold by Alice, $R_{B_{1}}$ is the revocation key for $B_{1}$ hold by Bob

Suppose Alice decides to pay Bob 0.25 mBTC (before this, each owns 1 mBTC):

1. Alice creates a new Bob's TX $B_{2}$
   - $B_{2}$: allocating 0.75 mBTC to Alice, and 1.25 mBTC to Bob
2. Alice signs $B_{2}$ and sends it to Bob
3. Bob receives $B_{2}$, signs it and keep it
4. Bob creates a new Alice's TX $A_{2}$
   - $A_{2}$: allocating 0.75 mBTC to Alice and 1.25 mBTC to Bob
5. Bob signs $A_{2}$ and sends it to Alice
6. Alice receives $A_{2}$, signs it and keep it
7. Alice provides $R_{A_{1}}$, which invalidates $A_{1}$. Then Alice can delete $A_{1}$
8. Bob provides $R_{B_{1}}$, which invalidates $B_{1}$. Then Bob can delete $B_{1}$

## Insights

- $A_{n}$ and $B_{n}$ are both 2-2 multisig TXs
  - $A_{n} = B_{n}$ if no one misbehaves
  - The fund is locked in this TX
- The latest TX (i.e. with bigger $n$) is the authority
- The TX $A_{i}/B_{i}$ are "updated" atomicly for each $i \to i+1$
- Neither Bob and Alice has incentive to fraud
  - Alice makes the payment. She wants to get something from Bob by paying Bob some Bitcoin. However, she signs the TX first so undeniable.
  - Bob receives the payment. He wants to get Bitcoin without giving Alice anything. Only when $A_{n}$ and $B_{n}$ are both broadcasted, the TX is finalized. Therefore, Hiding $A_{n}$ cannot make Bob profit.

# Closing the Duplex Payment Channel

- Good way (mutual close)
  - Alice and Bob generate a closing TX (which is similar to a commitment transaction, but without any pending payments) and publish it on the blockchain
- Bad way (unilateral close)
  - something goes wrong, possibly without evil intent on either side. One side publishes its latest commitment transaction.
    - e.g. one party crashed
- Ugly way (revoked TX close)
  - one of the parties deliberately tries to cheat, by publishing an **outdated** commitment transaction (presumably, a prior version, which is more in its favor).

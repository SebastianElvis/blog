---
title: Atomic Swap and Cross-chain Trading
date: 2018-12-25 10:08:00
categories:
- blockchain
- cross-chain
tags:
- Atomic Swap
- HTLC
---

# Introduction

Cross-chain Trading:
- Allows some amount of one cryptocurrency (e.g. Bitcoin) to be traded for some amount of another cryptocurrency (e.g. Bitcoin on a sidechain)

Hashed Timelock Contracts (HTLC):
- Use hash locks and timelocks to require a payee either
  - acknowledge receiving prior to a deadline by generating cryptographic proof of payment
  - or forfeit the ability to chaim the payment and return it to the payer
- Hashlock: 
  - a type of encumbrance 
  - restricts the spending of an output until **a specified piece of data is publicly revealed**.
- Timelock:
  - a type of smart contract primitive
  - restricts the spending of an output until a specified **future time** or **block height**.

# Hashed Timelock Contracts

## Hashlock

The Bitcoin Script describes a transaction that uses one of these hash functions to create a **transaction puzzle**
- A transaction output which can only be spent by someone who can satisfy this encumbrance

```
OP_HASH256 6fe28c0ab6f1b372c1a6a246ae63f74f931e8365e15a089c68d6190000000000 OP_EQUAL
```

Execution process:
1. take the data on the top of the stack (not shown)
2. hash it with the sha256d function (creating a computed hash)
3. compare it to the string 6fe2...0000 above (the provided hash)
4. If the computed hash equals the provided hash, then the encumbrance is satisfied and the output can be spent

- The Script page tells us that provided hash is the hash of the Genesis Block header
- so the data necessary to satisfy this encumbrance is the header of the Genesis Block. 
- Someone has already done that, so if we look up the transaction on the blockchain that spent this output, we would see that Genesis Block header in one of that transaction's scriptSigs.

## Timelock

### nLockTime

- A part of the original Bitcoin implementation
- *nLockTime* is a field that specifies the **earliest** time a transaction may be added to a valid block. 
- A later Bitcoin soft fork allowed *nLockTime* to alternatively specify the **lowest block height** a transaction may be added to a valid block.

### CheckLockTimeVerify (CLTV)

- In late 2015, the **BIP65** soft fork redefined the **NOP2** opcode as the **CheckLockTimeVerify (CLTV)** opcode
- allowing transaction outputs (rather than whole transactions) to be encumbered by a timelock
- When the CLTV opcode is called, it will cause the script to fail unless the nLockTime on the transaction $\geq$ the time parameter provided to the CLTV opcode. 
- Since a transaction may only be included in a valid block if its nLockTime is in the past, this ensures the CLTV-based timelock has expired before the transaction may be included in a valid block.
- CLTV is currently used in **CLTV-style payment channels**.

### Relative locktime

- In mid-2016, the **BIP68/112/113** soft fork 
- give consensus-enforced meaning to some values in the *nSequence* field, that is a part of every transaction input, creating a *relative locktime*. 
- This allowed an input to specify the **earliest time** it can be added to a block based on how long ago the output being spent by that input was included in a block on the block chain.

### CheckSequenceVerify (CSV)

- Also part of the **BIP68/112/113** soft fork 
- the *CheckSequenceVerify* opcode provides for *relative locktime* the same feature *CLTV* provides for absolute locktime
- When the *CSV* opcode is called, it will cause the script to fail unless the *nSequence* on the transaction indicates an equal or greater amount of relative locktime has passed than the parameter provided to the *CSV* opcode. 
- Since an input may only be included in a valid block if its relative locktime is expired, this ensures the CSV-based timelock has expired before the transaction may be included in a valid block.
- CSV is used by Lightning Network transactions.


## HTLCs in Payment Channels

Payment channels already use timelocks, and it can be relatively simple conceptually to extend them with hashlocks. This provides the useful benefit of making payments routable across two or more payment channels.

1. Alice opens a payment channel to Bob, and Bob opens a payment channel to Charlie.
2. Alice wants to buy something from Charlie for 1000 satoshis.
3. Charlie generates a random number $r$ and generates its SHA256 hash $h = H(r)$. Charlie gives $h$ to Alice.
4. Alice uses her payment channel to Bob to pay him 1,000 satoshis, but she adds $h$ to the payment along with an extra condition:
   - in order for Bob to claim the payment, he has to provide the data which was used to produce $h$.
5. Bob uses his payment channel to Charlie to pay Charlie 1,000 satoshis
    - **and** adds the same condition as Alice (the $h$ puzzle to Charlie).
6. Charlie has the original data that was used to produce the hash ($r$, called a pre-image), so Charlie can use it to finalize his payment and fully receive the payment from Bob. By doing so, Charlie necessarily makes the $r$ available to Bob.
7. Bob uses the $r$ to finalize his payment from Alice


# Atomic Swap

(At least) Two parties want to exchange different coins without having to trust a third party.

- A non-atomic trivial solution is: Alice send her coins to Bob, and then have Bob send other coins to Alice
  - but Bob has the option of going back on his end of the bargain and simply not following through with the protocol, ending up with both sets of coins.
- Atomic swaps can be used for
  - trading between bitcoin and another cryptocurrency
  - trading bitcoin UTXOs for **privacy** purposes (Coinswap).

Solution by using Contracts and *nLockTime*

- A picks a random number $x$, $h = H(x)$
- A creates TX1: "Pay $w$ BTC to $PK_{B}$ if ($x$ is known and signed by B) or (signed by A&B)"
- A creates TX2: "Pay $w$ BTC from TX1 to $PK_{A}$, locked for 48 hours, signed by A"
- A sends TX2 to B
- B signs TX2 and returns to A
**1. A submits TX1 to the Blockchain**
- B creates TX3: "Pay $v$ altcoins to $PK_{A}$ if ($x$ is known and signed by A) or (signed by A&B)"
- B creates TX4: "Pay $v$ altcoins from TX3 to $PK_{B}$, locked 24 hours, signed by B"
- B sends TX4 to A
- A signs TX4 and sends back to B
**2. B submits TX3 to the Blockchain**
**3. A spends TX3, revealing $x$**
**4. B spends TX1, using $x$**

Situations:

- Before 1: Nothing public has been broadcast, so nothing happens
- Between 1 & 2: A can use refund transaction after 72 hours to get his money back
- Between 2 & 3: B can get refund after 24 hours.  A has 24 more hours to get his refund
- After 3: Transaction is completed by 2
  - A must spend his new coin within 24 hours or B can claim the refund and keep his coins
  - B must spend his new coin within 72 hours or A can claim the refund and keep his coins

Disadvantages:
- if counterparty does not go through the trade, funds will be locked and cannot used in another trade for the duration of timeout. 
- it depends on transaction replacement which may, or may not be considered standard under current bitcoin protocol rules.
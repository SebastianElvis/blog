---
title: CCS'16 - Making Smart Contracts Smarter
date: 2018-12-15 11:18:03
categories:
- paper reading
tags:
- Smart Contracts
- Solidity
- Ethereum
---


# Info

- **Title**: Making Smart Contracts Smarter
- **Conference**: ACM CCS
- **Year**: 2016
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2016/633.pdf
- **Source code**: https://github.com/melonproject/oyente

# Addressed Problems

- Smart contracts has high enough financial incentives to attract adversaries.
- Ethereum is permissionless, so adversaries are arbitrary
- The security of smart contracts has not received much attention
- smart contracts are irreversible and immutable. 
  - There is no way to patch a buggy smart contract, regardless of its popularity or how much money it has

# Solution

- We document several new classes of security bugs in Ethereum smart contracts.
- We formalize the semantics of Ethereum smart contracts and propose recomendations as solutions for the documented bugs.
- We provide *Oyente*, a symbolic execution tool which analyses Ethereum smart contracts to detect bugs.
- We run *Oyente* on real Ethereum smart contracts and confirmed the attacks in the real Ethereum network.

# Methodology

## Security Bugs in ETH Smart Contracts

1. Transaction-Ordering Dependence
   - Users have uncertain knowledge of which state the contract is at when their individual invocation is executed.
   - We call such contracts as transaction-ordering dependent (TOD) contracts.
   - e.g. The Crypto Puzzle contract (PoW)
     - A malicious owner of the Puzzle contract can keep listening to the network to see if there is a transaction which submits a solution to his contract.
     - setting higher *gasPrice*
2. Timestamp Dependence
   - A contract may have uses the block timestamp as a triggering condition to execute some critical operations, e.g., sending money. 
   - We call such contracts as timestamp-dependent contracts.
   - e.g. The Run contract
     - The contract uses the timestamp and block number as the entropy source to generate a random number, which decides a participant to win the Jackpot
     - timestamp of a block can be arbitrary ~900s
     - The entropy source is not secure
3. Mishandled Exceptions
   - If there is an exception raised (e.g., not enough gas, exceeding call stack limit) in the callee contract, the callee contract terminates, reverts its state and returns false.
   - However, depending on how the call is made, the exception in the callee contract **may or may not** get propagated to the caller.
     - The caller contract should **explicitly** check the return value to verify if the call has been executed properly.
     - This inconsistent exception propagation policy leads to many cases where exceptions are not handled properly.
   - e.g. KingOfTheEtherThrone (KoET for short) contract
     - Participants bid to become the king
     - The KoET contract does not check the result of the compensation transaction
     - Rely on the instant notification of the exception handling and updated status
4. Reentrancy Vulnerability
   - The DAO hack
   - In Ethereum, when a contract calls another, the current execution waits for the call to finish. 
     - This can lead to an issue when the recipient of the call makes use of the intermediate state the caller is in.
     - The network is fully asynchronous and probablistic, so not deterministic

## Oyente

![](oyente.png)

CFG: Control Flow Graph

- Based on Symbolic Execution
  - because it can statically reason about a program path-by-path
  - superior to *dynamic testing*, which reasons about a program input-by-input

# Relevance

- Smart contracts is the core component for some protocols like Lightning Network
- Smart contract security is highly concerned currently

# To discuss/investigate

- Debug more on smart contracts
- Dissecting the EVM and smart contract programming language
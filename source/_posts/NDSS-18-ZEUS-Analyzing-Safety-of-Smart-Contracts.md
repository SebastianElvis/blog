---
title: 'NDSS''18 - ZEUS: Analyzing Safety of Smart Contracts'
date: 2019-01-03 19:56:32
categories:
- paper reading
tags:
- Smart Contracts
---




# Info

- **Title**: ZEUS: Analyzing Safety of Smart Contracts
- **Conference**: NDSS
- **Year**: 2018
- **URL**: http://wp.internetsociety.org/ndss/wp-content/uploads/sites/25/2018/02/ndss2018_09-1_Kalra_paper.pdf
- **Source code**: https://seahorn.github.io/

# Addressed Problems

- A smart contract is hard to patch for bugs once it is
deployed, irrespective of the money it holds. 
A recent bug caused losses worth around $50 million of cryptocurrency.

# Solution

- We classify several new and previously known issues but unstudied in the context of smart contracts and show that they can potentially lead to loss of money.
- We present a formal abstraction of Solidity’s execution semantics for verifying smart contracts using a combination
of abstract interpretation and symbolic model checking.
- We present the design and implementation of ZEUS, a symbolic model checking framework for verification of correctness and fairness policies.
- We present the first large scale source code analysis of
Solidity-based smart contracts.
- ZEUS is sound (with zero false negatives) and outperforms Oyente for contracts in our data set
- We show ZEUS’s generic applicability by leveraging it to verify smart contracts for the Fabric blockchain.

# Methodology

## Taxonomy

- Incorrect Contracts
  - REENTRANCY
    - A function is reentrant if it can be interrupted while in the midst of its execution, and safely re-invoked even before its previous invocations complete execution.
  - UNCHECKED SEND
    - Since Solidity allows only 2300 gas upon a send call, a computation-heavy fallback function at the receiving contract will cause the invoking send to fail.
  - FAILED SEND
    - executing a throw upon a failed send, in order to revert the transaction. However, this paradigm can also put contracts at risk.
  - INTEGER OVERFLOW/UNDERFLOW
    - all arithmetic operations to be susceptible to overflow/underflow
  - TRANSACTION STATE DEPENDENCE
- Unfair Contracts
  - ABSENCE OF LOGIC
  - INCORRECT LOGIC
  - LOGICALLY CORRECT BUT UNFAIR


## ZEUS

- Formalizing Solidity Semantics
- Formalizing the Policy Language
- The proof for soundness
- Symbolic Model Checking via CHCs

# Relevance

- Smart Contract Security

# To discuss/investigate

- This project seems not fully open-source
- LLVM based compiler seems to be promising
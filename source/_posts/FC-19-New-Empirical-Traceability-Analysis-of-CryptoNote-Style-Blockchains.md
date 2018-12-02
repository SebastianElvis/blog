---
title: FC'19 - New Empirical Traceability Analysis of CryptoNote-Style Blockchains
date: 2018-11-21 18:45:28
categories:
- paper reading
tags:
- Confidential Transactions
- CryptoNote
- FC
---



# Info

- **Title**: New Empirical Traceability Analysis of CryptoNote-Style Blockchains
- **Conference**: FC
- **Year**: 2019
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: No
- **Source code**: No

# Addressed Problems

- In practice, CryptoNote-style cryptocurrencies fall short from achieving their claimed anonymity
    - Monero transactions may be de-anonymized via statistical analysis
    - Most inputs in Monero have very small number of mixins and more than half inputs are paid without mixin
    - Once a coin payed without mixin is chosen as a mixin in another transaction, the input of this transaction also faces a danger of being de-anonymized.
        - called "chain-reaction" analysis or cascade effect [4].
- Problems still exist
    - Can anonymity of users be well-protected with a current small ring size, i.e., the counter-measures for known attacks?
    - How to theoretically analyze the security level achieved by those cryptocurrencies adopting the ring signature for untraceability
- There is no empirical analysis on other CryptoNote-style currencies.

# Solution

- We show that the current countermeasures to resist known attacks make Monero a good system to provide anonymity
- We show other CryptoNote-style protocols are still suffering from the same type of attacks.
- We introduce a new attack on the untraceability of CryptoNote-style currencies called **closed set attack**
    - This attack is based on the fact that $n$ transaction inputs will and must use $n$ distinct public-keys as real-spend, since each public-key can only be redeemed once.
    - A set of inputs is called a *closed set* if the number of inputs equals to the number of distinct public-keys included.
    - Hence, we can deduce that all public-keys included in a *closed set* must be mixins in other inputs outside of this closed set.
    - In this way, the searching for closed sets will be helpful to track the real-spend of some other inputs.
- We verify the impact of our attack via performing experiments on actual blockchain data, where we pick the top 3 CryptoNote-style currencies by market capitalization, i.e., Monero, Bytecoin and DigitalNote.
    - As the closed set attack is too expensive to run due to its high complexity, we propose an efficient algorithm, namely, **clustering algorithm**, to (approximately) implement closed set attack.
    - We give a lower bound of our clustering algorithm in implementing the closed set attack. Specifically, we prove that our algorithm can find all *closed set* of size 5.
- In addition, we also provide a theoretical analysis on the existence of *closed set*. 
    - We find that if all inputs have 3 mixins and all mixins are uniformly distributed, with all but a very small probability (about $2^{-19}$), there will not exist any *closed set*.

# Methodology



# Relevance

# To discuss/investigate

---
title: USENIX Security'18 - An Empirimal Analysis of Anonymity in Zcash
date: 2018-11-08 18:10:56
categories:
- paper reading
tags:
- USENIX Security
- Zcash
- Blockchain
- Security&Privacy 
---

# Info

- **Title**: An Empirimal Analysis of Anonymity in Zcash
- **Conference**: USENIX Security
- **Year**: 2018
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://arxiv.org/abs/1805.03180
- **Source code**: https://github.com/manganese/zcash-empirical-analysis


# Addressed Problems

- Bitcoin does not provide any meaningful level of anonymity, so a number of alternative cryptocurrencies or other PETs have been deployed to achieve the anonymity.
- Zcash has two types of transactions.
    - TXs in the *shielded pool* which conceal the coins spent
    - *Transparent* TXs which are essentially the same as Bitcoin TXs (with plaintext addresses and amounts)
    Still not perfectly anonymous
- No formal in-depth empirical analysis on Zcash was provided

# Solution

This paper provided the first in-depth empirical analysis of anonymity in Zcash, in order to 

- examine chaims in the Zcash paper
- reveal how Zcash has evolved 
- reveal its main participants

# Methodology

- **Section 4** provided a general examination of Zcash
    - observed that the vast majority of Zcash activity is in the transparent part of the blockchain
    - which means it does not engage with the shielded pool at all
- **Section 5** explored this aspect of Zcash
    - adapted the analysis that has already been developed for Bitcoin
    - found that exchanges typically dominate this part of the blockchain
- **Section 6** examinined interactions with *shielded pool*
    - found that the main actors doing so (interacting with *shield pool*) are the founders and miners, who are **required** to put all newly generated coins directly into it
    - Using newly developed heuristics for attributing transactions to founders and miners, we find that **65.6%** of the value withdrawn from the pool **can be linked back** to deposits made by either founders or miners. 
    - implemented a general heuristic for linking together other types of transactions, and capture an **additional 3.5%** of the value using this
    - In conclusion, our relatively simple heuristics thus reduce the size of the overall anonymity set by 69.1%
- **Section 7** look at the relatively small percentage of transactions that have taken place **within** *shielded pool*
    - found that **relatively little information can be inferred**, although we do identify certain patterns that may warrant further investigation
- **Section 8** and **Section 9** performed a small case study of the activities of the Shadow Brokers within Zcash


# Relevance

We evaluated the CryptoNote protocol, which implemented essentially the same thing as Zcash.
By understanding the underlying technology and potential attacks on Zcash, we may step further in the field of anonymous blockchain transactions.

# To discuss/investigate

- We may improve our evaluations on CryptoNote by adding statistics applied in this paper
- Maybe we can do a comparison between Monero/Zcash?
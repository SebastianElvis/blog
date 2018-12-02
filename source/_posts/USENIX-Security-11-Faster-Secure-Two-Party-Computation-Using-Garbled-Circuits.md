---
title: USENIX Security'11 - Faster Secure Two-Party Computation Using Garbled Circuits
date: 2018-12-02 15:22:50
categories:
- paper reading
tags:
- MPC
- Garbled Circuits
- USENIX Security
---


# Info

- **Title**: Faster Secure Two-Party Computation Using Garbled Circuits
- **Conference**: USENIX Security
- **Year**: 2011
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://www.usenix.org/event/sec11/tech/full_papers/Huang.pdf
- **Source code**: https://github.com/uvasrg/FastGC

# Addressed Problems

- There are two main approaches to constructing protocols for secure computation.
  - The first approach exploits specific properties of $f$ to design special-purpose protocols that are, presumably, more efficient than those that would result from generic techniques.
  - The second approach relies on completeness theorems for secure computation [7, 8, 34] which give protocols for computing any function $f$ starting from a Boolean circuit representation of $f$.
- Indeed, some previous works have explicitly rejected garbled-circuit solutions due to memory exhaustion [16, 26].
  - In previous garbled-circuit implementations including *Fairplay*, the garbled circuit (whose length is several hundreds bits per binary gate) is fully generated and loaded in memory before circuit evaluation starts. This impacts both the **efficiency** of the resulting implementation and **severely limits its scalability**.
  - Developing and debugging privacy-preserving applications using existing compilers is **tedious, cumbersome, and slow**.
  - The high-level programming abstraction provided by *Fairplay* and other tools for secure computation obscures important opportunities for generating more compact circuits


# Solution

- We show a general method for implementing privacy-preserving applications using garbled circuits that is both faster and more scalable than previous approaches.
- We observe that it is **unnecessary** to generate and store the entire garbled circuit at once. 
  - By topologically sorting the gates of the circuit and pipelining the process of circuit generation and evaluation we can significantly improve overall efficiency and scalability.
  - We also employ all known optimizations, including the “free XOR” technique [18], garbled-row reduction [27], and oblivious-transfer extension [14].
- We present a new method and supporting framework for generating efficient protocols for secure two-party computation.
  - Our method enables programmers to generate a secure protocol computing some function $f$ from an existing (insecure) implementation of $f$, while providing enough control over the circuit design to enable key optimizations to be employed.
  - Our approach allows users to write their programs using a combination of high-level and circuit-level Java code.

# Methodology

## Threat Model

- We adopt the semi-honest (also known as honest-but-curious) threat model, where parties are assumed to follow the protocol but may attempt to learn additional information about the other party’s input from the protocol transcript.
- Further, our implementation could be modified easily so as to give meaningful privacy guarantees even against malicious adversaries.
- Note that this usage of our protocols provides privacy, but does not provide any correctness guarantees.
  - A malicious generator could construct a circuit that produces an incorrect result without detection.
- Many interesting privacy- preserving applications do have the properties needed for our approach to be effective.
  1. both parties have a **motivation** to produce the correct result
  2. only one party needs to receive the output. 
  3. e.g. 
    - include financial fraud detection (banks cooperate to de- tect fraudulent accounts)
    - personalized medicine (a patient and drug company cooperate to determine the best treatment) 
    - privacy-preserving face recognition

## Implementation

- Pipelined Circuit Execution
- Circuit-level Representation Optimization (Compact Circuits)
- The circuit library and how a programmer defines a new circuit component
- implementation parameters

## Real World Examples

- Hamming Distance
- Levenshtein Distance
- Smith-Waterman Algorithm for genome and protein alignment
- AES

# Relevance

- GC may be a way of implementing Confidential Transactions

# To discuss/investigate

- GC seems to be a good topic for applied crypto research, maybe we can go further on this direction?
- Is 2PC worth doing?
---
title: >-
  Journal of Cryptology'17 - A Full Proof of the BGW Protocol for Perfectly Secure Multiparty Computation
date: 2018-11-30 18:10:58
categories:
- paper reading
tags:
- MPC
- VSS
- BGW Protocol
---



# Info

- **Title**: A Full Proof of the BGW Protocol for Perfectly Secure Multiparty Computation
- **Conference**: Journal of Cryptology
- **Year**: 2017
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://eprint.iacr.org/2011/136.pdf
- **Source code**: No

# Addressed Problems

- Ben-Or, Goldwasser and Wigderson (BGW) Protocol is one of the most fundamental results of MPC
  - Any n-party functionality can be computed with *perfect security*, in the *private channel model*.
  - When the adversary is *semi-honest* this holds as long as t < n/2 parties are corrupted
  - When the adversary is malicious this holds as long as t < n/3 parties are corrupted.
- Unfortunately, a full proof of these results was never published.

# Solution

- In this paper, we remedy this situation and provide a full proof of security of the BGW protocol.

# Primitives

## Shamir Secret Sharing

A *(t+1)-out-of-n* secret sharing scheme:
- Input: a secret $s$
- Outputs: $n$ shares
  - Can only reconstruct $s$ from every subset of $t+1$ shares
  - $t$ is called the **thereshold** of the scheme

SSS consists of 2 algorithms:
- The **sharing algorithm**
- The **reconstruction algorithm**

Process of SSS:
- Pre-defined stuff:
  - $\mathbb{F}$ a finite field of size greater than $n$, and let $s \in \mathbb{F}$ 
- Sharing
  - Defines a polynomial $q(x)$ in $\mathbb{F}[x]$
    - degree (highest degree of individual terms with non-zero coefficients) is $t$
    - the constant term is $s$
    - all other coefficients are selected uniformly, independently and randomly in $\mathbb{F}$
  - The shares are defined as $q(\alpha_{i})$ for every $i \in \{1,...,n\}$
    - where $\alpha_{1},...\alpha_{n}$ are any $n$ distinct non-zero predetermined values in $\mathbb{F}$
- Reconstruction
  - Based on the fact that any $t+1$ points define exactly one polynomial of degree $t$
  - Using interpolation can efficiently reconstruct $q(x)$ given any subsets of $t+1$ points $(\alpha_{i}, q(\alpha_{i})$

## Verifiable Secret Sharing based on SSS

- The dealer inputs a polynomial $q(x)$ of degree $t$
- Each party $P_{i}$ receives its Shamir share $q(\alpha_{i})$ based on that polynomial.
- The "verifiable" part is that if $q$ is of degree greater than $t$, the parties reject the dealer's shares and output $\perp$

## Securely Computing $F_{VSS}$

- Input: The dealer $D = P_{1}$ holds a poly $q(x)$ of degree at most $t$. The other parties $P_{2},...,P_{n}$ have not inputs
- Public values: The description of a field $\mathbb{F}$ and $n$ non-zero elements $\alpha_{1},...,\alpha_{n} \in \mathbb{F}$
- The Protocol:
  1. **Send Shares** - The dealer $D$
    1.1. $D$ selects a uniformly distributed bivariate polynomial $S(x,y) \in \mathcal{B}^{q(0),t}$, where $S(0,z) = q(z)$
    1.2. For every $i \in \{1,...,n\}$, $D$ defines
      - polynomial $f_{i}(x) \stackrel{def}{=} S(x,\alpha_{i})$
      - polynomial $g_{i}(y) \stackrel{def}{=} S(\alpha_{i}, y)$
    1.3. $D$ then sends to each party $P_{i}$ the poly $f_{i}(x)$ and $g_{i}(y)$
  2. **Exchange Subshares** - Each party $P_{i}$
    2.1. $P_{i}$ stores the poly $f_{i}(x)$ and $g_{i}(y)$ received from $D$
      - If $f_{i}(x)$ or $g_{i}(y)$ is of degree greater than $t$ then truncate it to be of degree $t$.)
    2.2. For every $j \in \{1,...,n\}$, $P_{i}$ sends $f_{i}(\alpha_{j})$ and $g_{i}(\alpha_{j})$ to $P_{j}$
  3. **Broadcast Compliants** - Each party $P_{i}$
    3.1. For every $j \in \{1,...,n\}$, let $u_{j}, v_{j}$ denote the values received from player $P_{j}$ in Round 2
      - these are supposed to be $u_{j} = f_{j}(\alpha_{i})$ and $v_{j} = g_{j}(\alpha_{i})$
      - If $u_{j} \neq f_{j}(\alpha_{i})$ or $v_{j} \neq g_{j}(\alpha_{i})$, broadcast complaint($i,j,f_{i}(\alpha_{j}, g_{i}(\alpha_{j}))$)
  4. **Resolve Complaints** - The dealer $D$: for every complaint message recevied, do:
    4.1. Upon viewing a message complaint $(i,j,u,v)$ broadcast by $P_{i}$, check that $u = S(\alpha_{j},\alpha_{i})$ and $v = S(\alpha_{i},\alpha_{j})$.
      - Note that if the dealer and $P_{i}$ are honest, then it holds that $u = f_{i}(\alpha_{j})$ and $v = g_{i}(\alpha_{j})$ 
    4.2. If the above condition holds, then do nothing. Otherwise, broadcast reveal($i,f_{i}(x), g_{i}(y)$)
  5. **Evaluate Complaint Resolutions** – Each party $P_{i}$
    - Haven't understood yet...
  6. **Output Decision** (if there were complaints) - Each party $P_{i}$
    6.1. If at least $n−t$ parties broadcast consistent, output $f_{i}(0)$. Otherwise, output $\perp$.


# Relevance

- VSS is a simple but powerful primitive, which may be very useful in Blockchain
- MPC is strongly related to the consensus problem

# To discuss/investigate

- Is it worth a try to combine VSS or Threshold Encryption to the consensus?
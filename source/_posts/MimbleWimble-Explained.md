---
title: MimbleWimble Explained
date: 2019-01-01 14:12:05
categories:
- blockchain
- privacy
tags:
- Cryptography
- Confidential Transactions
---

# Addresses Problems

- All Bitcoin participants must download and **replay** every tx that has ever occurred.
- Bitcoin blockchain is huge
  - Today the Bitcoin blockchain sits just shy of 100Gb on the author’s disk. 
  - With Confidential Transactions[Max15] (CT), each of the approximately 430 million outputs would consume 2.5Kb each, totalling over a terabyte of historical data.

# Solutions

- we describe a research project to design a Bitcoin-like cryptocurrency, which achieves dramatically better scaling and privacy properties than Bitcoin.
  - It allows removal of most historic blockchain data while still allowing users to fully verify the chain, including 
    - spent transaction outputs
    - rangeproofs
    - other stuff...
  - Further, this currency allows transactions to be noninteractively combined (e.g. Coinjoin[Max13a]) and cut-through[Max13b], eliminating much of the topological structure of the transaction graph.
  - Unfortunately, it accomplishes these goals at cost of sacrificing functionality.


# Methodology

## Design goals

Sacrifices:

- Mimblewimble cannot support a general-purpose scripting system such as that in Bitcoin, which precludes
  - zero-knowledge contingent payments[Max16]
  - cross-chain atomic swaps[Nol13]
  - micropayment channels[PD16]

Achievements:

- Direct transfer of value between parti(es) to other parti(es)
- Output amounts are hidden
- Transactions should be noninteractively aggregable[Mou13]
- For a new participant, the amount of bandwidth and processing power needed to catch up with the system should be proportional to the current state of the system


## Cryptography Primitives

### Groups

- $\mathscr{Y}_{1}, \mathscr{Y}_{2}$ are Elliptic Curve Groups 
  - with an efficiently computable Bilinear Pairing $\hat{e}: \mathscr{Y}_{1} \times \mathscr{Y}_{2} \rightarrow \mathscr{Y}_{T}$
  - with $\mathscr{Y}_{T}$ equal to the multiplicative group of $\mathbb{F}_{q^{k}}$ for some prime $q$, small positive integer $k$

### Commitment

- A Commitment Scheme is a pair of algorithms Commit($v, r$) $\rightarrow$ $\mathscr{Y}$, Open($v, r$, C) $\rightarrow$ {true, false}
- s.t. Open($v, r$, Commit($v, r$)) = true

- We define a Homomorphic Commitment Scheme that
  - commitments to $v$, $v'$ can be added to obtain a commitment to $v+v'$ having the same security properties

e.g. Pedersen Commitment

- Commitment $\mathbb{Z}_{r}^{2} \rightarrow \mathscr{Y}$: Commit($v, r$) = C = $vH + rG$
- Open $\mathbb{Z}_{r}^{2} \times \mathscr{Y} \rightarrow$ {true, false}: Open($v, r$, C) = ($vH+rG \stackrel{?}{=} C$) = true

### Rangeproof

Given a Homomorphic Commitment $C$, we define a rangeproof on $C$ as a cryptographic proof that the commitment value of $C$ lies in some given range [a, b].

The rangeproofs are zero-knowledge proofs of knowledge (zkPoK) of the opening info of the commitments.

### Sinking Signatures

We define a sinking signature as the following collection of algorithms:

- Setup($1^{\lambda}$) $\rightarrow$ a keypair (sk, pk)
- Sign(sk, $h$) takes the secret key $sk$ and non-integer height $h$ which is a polynomial in $\lambda$, and outputs a signature $s$
- Verify($pk, h, s$) outputs {true, false}

The name "sinking signature" is motivated by the fact that 
- given a signature $s$ on height $h$, it may be possible for a forger to create a signature $s'$ on height $h'$ with the same $pk$ and $h' \leq h$, thus **decreasing the height** of the signature.


We say a sinking signature is **aggregatable** or **summable** if given a a linear combination $pk$ of $pk_{i}$ values computed from Setup$(1^{\lambda})$, the same linear combination of $s_{i} \leftarrow Sign(sk_{i},h)$ (for fixed $h$) it is possible to compute a signature s such that $Verify(pk,h,s)$ accepts.
(similar to the batch verification)


## MimbleWimble Primitives

### Transaction

A MimbleWimble TX contains the following data:

- A list of Homomorphic Commitments termed the **inputs**, with attached rangeproofs.
- A list of Homomorphic Commitments termed the **outputs**, with attached rangeproofs.
- A blockheight $x$
- An excess commitment to zero, with a summable sinking signature on blockheight $x$ with this as pubkey

We define the $sum = outputs - inputs + excess$.

The TX is valid if
- $sum = 0$
- the rangeproofs and sinking signature are valid

#### Ownership

We say that a party S owns a set of TX outputs if she knows the **opening** of the sum of the outputs

#### Sending n coins

To send $n$ coins from $S$ to $R$, $S$ produces a TX:
- chooses inputs
- creates uniformly random change output(s)
- a uniformly random excess
- where sum is a commitment to $n$
- S sends the stuff above along with the opening info of the sum

#### Receiving n coins

To receive $n$ coins, $R$ receives a TX $T'$ which sums to a commitment of $m \geq n$ coins, along with opening info of the sum.
1. $R$ first produces uniformly random outputs whose total committed value is $n$, and adds these to the TX
2. If $m = n$, $R$ negates the sum of this TX and adds it to the excess of the TX, so that the total sum is now zero
3. Otherwise, the TX now has sum committing to $m-n$, so $R$ adds a uniformly random value to excess, updates the signature, and gives the opening info of the new sum to another recipient who can take the remaining value.

 

#TODO!!!



# Reference

1. https://download.wpsoftware.net/bitcoin/wizardry/mimblewimble.pdf
2. https://download.wpsoftware.net/bitcoin/wizardry/mimblewimble.txt
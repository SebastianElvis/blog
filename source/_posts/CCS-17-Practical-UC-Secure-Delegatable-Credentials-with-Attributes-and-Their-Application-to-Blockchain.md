---
title: CCS'17 - Practical UC-Secure Delegatable Credentials with Attributes and Their Application to Blockchain
date: 2018-11-20 18:47:04
categories:
- paper reading
tags:
- Delegatable Anonymous Credentials
- Universal Composability
- CCS
---



# Info

- **Title**: Practical UC-Secure Delegatable Credentials with Attributes and Their Application to Blockchain
- **Conference**: ACM CCS
- **Year**: 2017
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://acmccs.github.io/papers/p683-camenischA.pdf
- **Source code**: No

# Addressed Problems

- **Delegatable anonymous credentials (DAC)** can allow a credential owner to delegate her credential to another user, who, in turn, can delegate it further as well as present it to a verifier for authentication purposes.
- Existing constructions are **not practical**, because they use generic zero-knowledge proofs or Groth-Sahai proofs with many expensive pairing operations and a large credential size.
- Existing constructions are described mostly in a **black-box** fashion (to hide the complexity of their concrete instantiations), often **leaving out the details** that would be necessary for their implementation.
- The existing DAC security models **do not consider attributes**, which, however, are necessary in many practical applications.
- Existing constructions **do not provide an ideal functionality** for DAC ([20]) or **are proven secure in standalone models** ([3, 17, 26]) that guarantee security only if a protocol is run in isolation

# Solution

- We propose **the first practical DAC system with attributes** that is well-suited for real-world applications.
  - A simple and ideal functionality $\mathcal{F}_{dac}$
  - Attributes can be different on any level ofdelegation.
  - Each attribute at any level can be selectively revealed when generating presentation token.
  - Tokens can be used to sign arbitrary messages.
  - Privacy is guaranteed only during presentation, during delegation the delegatee knows the full credential chain delegated to her.
- We propose **a generic DAC construction from signature schemes and zero-knowledge proofs** and **prove it secure** in the **universal composability (UC) framework** introduced by Canetti [14].
- We describe **a very efficient instantiation of our DAC scheme** based on a recent structure-preserving signature scheme by Groth [29] and on Schnorr zero-knowledge proofs [34].
- We report on an **implementation** of our scheme in the context of **a privacy-preserving membership service for permissioned blockchains** and give **concrete performance figures**, demonstrating the practicality of our construction.

# Primitives

## Bilinear Map

Let $\mathcal{G}$ be a bilinear group generator that takes as an input a security parameter 1κ and outputs the descriptions of multiplicative groups $ \Lambda = (q, \mathbb{G}_{1}, \mathbb{G}_{2}, \mathbb{G}_{t}, e, g_{1}, g_{2}) $, where:

- $\mathbb{G}_{1}, \mathbb{G}_{2}, \mathbb{G}_{t}$ are groups of prime order $q$
- $e$ is an efficient, non-degenerating bilinear map $e (\mathbb{G}_{1} \times \mathbb{G}_{2}) \rightarrow \mathbb{G}_{t}$
- $g_{1}, g_{2}$ are generators of groups $\mathbb{G}_{1}, \mathbb{G}_{2}$ respectively
- We denote $ \Lambda^{*} = (q, \mathbb{G}_{1}, \mathbb{G}_{2}, \mathbb{G}_{t}, e) $ as $ \Lambda $ without group generators

## Zero-Knowledge Proofs

*Prover* proves he knows $(a,b,c)$ to the *verifier* without leaking $(a,b,c)$.

$ PK\{ (a,b,c) : Y = g_{1}^{a} \wedge \tilde{Y} = \tilde{g_{1}}^{a}\tilde{H}^{c} \} $ denotes a

-  *zero-knowledge **Proof of Knowledge** of integers $a,b,c$ * s.t. 
  -  $Y = g_{1}^{a}H^{b}$ and $ \tilde{Y} = \tilde{g_{1}}^{a}\tilde{H}^{c} $
  -  where $y, g, h$ are elements of some groups $G = \langle g_{1} \rangle = \langle H \rangle$
  -  where $\tilde{Y}, \tilde{g_{1}}, \tilde{H}$ are elements of some groups $\tilde{G} = \langle \tilde{g_{1}} \rangle = \langle \tilde{H} \rangle$
- $(a,b,c)$ denote the knowledge, while all other values are known to the verifier

We can create a similar proof proving knowledge of group elements instead of exponents, e.g.

$SPK\{a \in \mathbb{G}_{1} : y = e(a, b) \}$ where:
- e(a, b) is a defined computation on the group $\mathbb{G}_{1}$, like $b^{a}$ on the finite field

We use $NIZK\{w, s(w)\}$ to denote a generic non-interactive zero-knowledge (NIZK) proof proving knowledge of witness $w$ such that statement $s(w)$ is true.

## Signature Schemes

A digital signature scheme $Sig$ is a set of PPT algorithms $Sig = (Setup, Gen, Sign, Verify)$:

- $Sig.Setup(1^{K}) \stackrel{\$}{\rightarrow} sp$: The setup algorithm $Sig.Setup$ takes a security parameter $1^{K}$ and outputs public system parameters $sp$ that also specify a message space $\mathcal{M}$.
- $Sig.Gen(sp) \stackrel{\$}{\rightarrow} (sk,vk) $: The key generation algorithm $Sig.Gen(sp)$ the input system parameter $sp$ and outputs a verification key $vk$ and a corresponding secret key $sk$.
- $Sig.Sign(sk,m) \stackrel{\$}{\rightarrow} \sigma $: The signing algorithm $Sig.Sign$ takes a private key $sk$ and a message $m \in \mathcal{M}$ and outputs a signature $\sigma$.
- $Sig.Verify(vk,m,\sigma) \rightarrow 1/0$: The verification algorithm $Sig.Verify$ takes a public verification key $vk$, a messagem $m$ and a signature $\sigma$ and outputs 1 for acceptance or 0 for rejection

## Sibling Signatures

Sibling Signature allows a signer with one key pair to use two different signing algorithms, each with a dedicated verification algorithm.
A sibling signature scheme consists of algorithms $(Setup, Gen, Sign1, Sign2, Verify1, Verify2)$.

- $Sib.Setup(1^{K}) \stackrel{\$}{\rightarrow} sp$: The setup algorithm $Sib.Setup$ takes a security parameter $1^{K}$ and outputs public system parameters $sp$ that also specify two message spaces $\mathcal{M}_{1}, \mathcal{M}_{2}$.
- $Sib.Gen(sp) \stackrel{\$}{\rightarrow} (sk, vk)$ : The key generation algorithm takes public system parameters $sp$ and outputs a verification key $vk$ and a corresponding secret key $sk$.
- $Sib.Sign1(sk,m) \stackrel{\$}{\rightarrow} \sigma$ : The signing algorithm $Sib.Sign1(sk,m)$ takes as input a private key $sk$ and a message $m \in \mathcal{M}$ and outputs a signature $\sigma$.
- $Sib.Sign2(sk,m) \stackrel{\$}{\rightarrow} \sigma$ : The signing algorithm $Sib.Sign2(sk,m)$ takes as input a private key $sk$ and a message $m \in \mathcal{M}$ and outputs a signature $\sigma$.
- $Sib.Verify1(vk,m,\sigma) \rightarrow 1/0$: The verification algorithm $Sib.Verify1$ takes as input a public verification key $vk$, a message $m$ and a signature $\sigma$ and outputs 1 for acceptance or 0 for rejection.
- $Sib.Verify2(vk,m,\sigma) \rightarrow 1/0$: The verification algorithm $Sib.Verify2$ takes as input a public verification key $vk$, a message $m$ and a signature $\sigma$ and outputs 1 for acceptance or 0 for rejection.

## Universal Composability (UC)

UC is a framework for proving a protocol is secure in the real world.

- In UC, an environment $\varepsilon$ gives input to the protocol participants and receives their outputs
- UC defines 2 world:
  - Real world
    - Honest parties execute a protocol over a network controlled by an adversary $\mathcal{A}$
    - $\mathcal{A}$ also controls the corrupt parties while communicating freely with environment $\varepsilon$
  - Ideal world
    - Honest parties are "dummy parties" who forward their inputs to the ideal functionality $\mathcal{F}$
    - $\mathcal{F}$ internally performs the desired task and generates outputs for honest parties
- A protocol $\pi$ securely realizes an ideal functionality $\mathcal{F}$ if the real world is as secure as the ideal world.
- For every adversary $\mathcal{A}$ attacking the real world, there exists a simulator $\mathcal{S}$ that performs an equivalent attack on the ideal world.
- If there are no meaningful attacks **on the ideal world**, it follows that there are no meaningful attacks **on the real world**.

# Methodology



# Relevance

- This is a plausible approach for access control in permissioned Blockchain systams. 
- We may explore its usage in public Blockchains.

# To discuss/investigate

- Is it possible to combine this technique with multi-signature?
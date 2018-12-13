---
title: Oakland'17 - Scalable Bias-Resistant Distributed Randomness
date: 2018-12-05 19:48:33
categories:
- paper reading
tags:
- Distributed Randomness
- MPC
---



# Info

- **Title**: Scalable Bias-Resistant Distributed Randomness
- **Conference**: IEEE S&P
- **Year**: 2017
- **URL**: https://eprint.iacr.org/2016/1067.pdf
- **Source code**: https://github.com/dedis/cothority

# Addressed Problems

## Background

- A reliable source of randomness that provides high-entropy output is a critical component in many protocols.
- More concretely, Tor hidden services [25] depend on the generation of a fresh random value each day for protection against popularity estimations and DoS attacks [34].
- The process of generating public randomness is nontrivial, because obtaining access to sources of good randomness, even in terms of entropy alone, is often difficult and error-prone [19], [36].
- Generating public randomness is hard, as active adversaries can behave dishonestly in order to bias public random choices toward their advantage.
    - e.g., by manipulating their own explicit inputs or by selectively injecting failures.

# Solution

- Our goal is to provide bias-resistant public randomness in the familiar (t, n)-threshold security model already widely-used both in threshold cryptography [24], [47] and Byzantine consensus protocols [15].
- We introduce two scalable public-randomness generation protocols
    - **RandHound** is a “one-shot” protocol to generate a single random output on demand.
    - **RandHerd** is a randomness beacon protocol that produces a regular series of random outputs. 
    - Both protocols provide the same key security properties of unbiasability, unpredictability, availability, and third-party verifiability of their random outputs.
- RandHound is a client-server randomness scavenging protocol enabling a client to gather fresh randomness on demand from a potentially large set of nearly-stateless randomness servers, preferably run by independent parties.
- RandHerd is a complementary protocol enabling a potentially large collection of servers to form a distributed public randomness beacon, which proactively generates a regular series of public random outputs.

# Methodology

## Publicly Verifiable Secret Sharing (PVSS)

## Schnorr (Multi-)Signature Scheme

- Threshold Signature Scheme (TSS)
    - A (t, n) TSS allows any subset of t signers to produce a valid signature.
- Collective Signing
    - Scales Schnorr Multi-Signature to thousands of participants by using aggregation techniques and communication trees

## RandShare

RandShare is an unbiasable randomness protocol that ensures unbiasability, unpredictability, and availability, but is practical only at small scale due to $O(n^{3})$ communication overhead.

## RandHound

RandHound, a scalable client/server protocol for producing public, verifiable, unbiasable randomness. RandHound enables a client, who initiates the protocol, to “scavenge” public randomness from an arbitrary collection of servers.

## RandHerd

RandHerd, a protocol that builds a collective authority or cothority [58] to produce unbiasable and verifiable randomness. RandHerd serves as a decentralized randomness beacon [45], [49], efficiently generating a regular stream of random outputs.

# Relevance

- Distributed Randoness may be useful for public Blockchains
- Generating a verifiable random strings across decentralized servers is similar to a consensus process

# To discuss/investigate

- Pay more attention to those primitives used in these protocols?
---
title: 'Oakland''17 - Hijacking Bitcoin: Routing Attacks on Cryptocurrencies'
date: 2019-01-03 19:14:31
categories:
- paper reading
tags:
- Attack
- Bitcoin
---


# Info

- **Title**: Hijacking Bitcoin: Routing Attacks on Cryptocurrencies
- **Conference**: USENIX Security
- **Year**: 2017
- **URL**: https://btc-hijack.ethz.ch/files/btc_hijack.pdf
- **Source code**: https://github.com/nsg-ethz/hijack-btc

# Addressed Problems

- One important attack vector of Cryptocurrencies has been left out: attacking the currency via the Internet routing infrastructure itself.

# Solution

- This paper presents the first taxonomy of routing attacks and their impact on Bitcoin, considering both
  - small-scale attacks
  - targeting individual nodes
  - large-scale attacks
  - targeting the network as a whole
- two key properties make routing attacks practical:
  - the efficiency of routing manipulation
  - the significant centralization of Bitcoin in terms of mining and routing
- We demonstrate the feasibility of each attack against the
deployed Bitcoin software
- we provide both short and long-term countermeasures, some of which can be deployed immediately

# Methodology

## BGP

- BGP (Border Gateway Protocol) [42] is the de-facto routing protocol that regulates how IP packets are forwarded in the Internet. (based on TCP)
- **BGP Hijack** An attacker wishes to attract all the traffic for a legitimate prefix $p$ (say, 100.0.0.0/16) by hijacking
  - For instance, in order to attract all traffic destined to $p$, the attacker could advertise 100.0.0.0/17 and 100.0.128.0/17.
  - By default, hijacking a prefix creates a black hole at the attacker’s location.

## Routing Attacks

### Partitioning the Bitcoin network

- In this attack, an AS-level adversary seeks to isolate a set of nodes P from the rest of the network, effectively partitioning the Bitcoin network into two disjoint components.
- The actual content of P depends on the attacker’s objectives and can range from one or few merchant nodes, to a set of nodes holding a considerable percentage of the total mining power.

### Delaying the propagation of blocks

- The actual content of P depends on the attacker’s objectives and can range from one or few merchant nodes, to a set of nodes holding a considerable percentage of the total mining power.
- Unlike partitioning attacks though, an attacker can delay the overall propagation of blocks towards a node even if she intercepts a subset of its connections.


## Countermeasures

### Short-term

- Increase the diversity of node connections
- Select Bitcoin peers while taking routing into account
- Monitor round-trip time (RTT)
- Monitor additional statistics
- Embrace churn
- Use gateways in different ASes
- Prefer peers hosted in the same AS and in /24 prefixe

### Longer-term

- Encrypt Bitcoin Communication and/or adopt MAC
- Use distinct control and data channels
- Use UDP heartbeats
- Request a block on multiple connections

# Relevance

- P2P layer security is a critical part of the Blockchain security

# To discuss/investigate

- What's the difference between this attack and the eclipse attack?
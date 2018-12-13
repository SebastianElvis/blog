---
title: DSN'18 - Byzantine Fault-Tolerant Atomic Multicast
date: 2018-12-05 19:44:35
categories:
- paper reading
tags:
- BFT
- Atomic Multicast
---


# Info

- **Title**: Byzantine Fault-Tolerant Atomic Multicast
- **Conference**: IEEE DSN
- **Year**: 2018
- **URL**: http://www.inf.usi.ch/faculty/pedone/Paper/2018/2018DSN.pdf
- **Source code**: No

# Addressed Problems

- Atomic multicast is a fundamental communication abstraction in the design space of strongly consistent distributed systems.
- Although research on efficient atomic multicast protocols is relatively mature [8], [9], [10], [11], to date all existing protocols target benign failures (e.g., crash failures) [12], [13], [14], [15].

# Solution

- We present a partially genuine atomic multicast protocol that builds on multiple instances of atomic broadcast, a problem that has been extensively studied and efficient libraries exist (e.g. [23], [24], [25]).
- We define the problem of building an overlay tree as an optimization problem.
    - Our optimization model takes into account the frequency of messages per destination and the performance of a group alone.
- We describe a prototype of ByzCast developed using BFT-SMaRt, a well-established library that implements BFT atomic broadcast.
- We provide a detailed experimental evaluation of ByzCast and compare it to a naive atomic multicast solution.

# Methodology

## System Model

- Processes, Groups and Communication
    - An unbounded set of client processes $ \mathcal{C} = \{c_{1}, c_{2},...\}$
    - A bounded set of server processes $ \mathcal{S} = \{p_{1},p_{2},...\} $
    - Clients and servers are disjoint
    - Client and server processes can be *correct* or *faulty*.
    - We define $ \Gamma = \{g_{1}, ..., g_{m}\}$ as the set of server process groups in the system. 
    - Groups are disjoint, non-empty, and satisfy $\cup_{g \in \Gamma} g = \mathcal{S}$.
    - Each group contains $3f+1$ processes, where $f$ is the maximum number of faulty server processes per group.
- Atomic Multicast
    - For every msg $m$, $m.dst$ denotes the groups to which $m$ is multicast.
    - A process atomically multicasts a message $m$ by invoking primitive *a-multicast($m$)* and delivers $m$ with *a-deliver($m$)*.
    - An atomic multicast algorithm $\mathcal{A}$ is genuine iff for any admissible run $R$ of $\mathcal{A}$ and for any correct process $p$ in $R$, if $p$ sends or receives a message, then some message $m$ is *a-multicast*, and either 
        - (a) $p$ is the process that a-multicasts $m$
        - (b) $p \in g$ and $g \in m.dst$ [22]

#TODO...

# Relevance

- Atomic multicast is useful for information propagation in Blockchain

# To discuss/investigate

- It may be interesting to reproduce this experiment?
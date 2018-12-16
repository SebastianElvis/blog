---
title: USENIX Security'15 - Eclipse Attacks on Bitcoin's Peer-to-Peer Network
date: 2018-12-16 12:24:30
categories:
- paper reading
tags:
- Bitcoin
- Attack
---

# Info

- **Title**: Eclipse Attacks on Bitcoin's Peer-to-Peer Network
- **Conference**: USENIX Security
- **Year**: 2015
- **URL**: https://eprint.iacr.org/2015/263.pdf
- **Source code**: https://github.com/bitcoin/bitcoin/pull/9037

# Addressed Problems

- Little attention has been paid to the P2P network security
- Cryptographic authentication between peers is not used, and nodes are identified by their IP addresses
- The P2P network is open, so adversaries are easy to get in

# Solution

- We present and quantify the resources required for **eclipse attacks** on nodes with public IPs running bitcoind version 0.9.3
- The attacker monopolizes all of the victim’s incoming and outgoing connections, thus **isolating** the victim from the rest of its peers in the network
- We present **off-path attacks**, where the attacker controls endhosts, but not key network infrastructure between the victim and the rest of the bitcoin network.
- We propose a set of countermeasures that preserve openness by allowing unsolicited incoming connections, while raising the bar for eclipse attacks.

# Methodology

## Implication of Eclipse Attack

Attacks based on Eclipse Attack
- Engineering block races
  - Block race: Two miners discover blocks at the same time
  - An attacker that eclipses many miners can engineer block races by hording blocks discovered by eclipsed miners, and releasing blocks to both the eclipsed and non-eclipsed miners once a competing block has been found
  - Thus, the eclipsed miners waste effort on orphan blocks.
- Splitting mining power
- Selfish mining
  - Attacker strategically withholds blocks to win more than its fair share of mining rewards
  - If attacker's blockchain is longer, withhold
  - If honest blockchain becomes longer, broadcast the attacker's blockchain and let them mine on the attacker's blockchain
  - Decrease the waste of mining power
  - http://www.greywyvern.com/code/javascript/selfishmining
- 0-confirmation double spend
  - Attacker eclipses the merchant's bitcoin node

## Bitcoin P2P Network

- A node with a public IP can initiate up to 8 outgoing connections with other bitcoin nodes, and accept up to 117 incoming connections (**Configurable**).
- Propagating Network Info
  - DNS Seeders
    - A DNS seeder is a server that responds to DNS queries from bitcoin nodes with a (**not** cryptographically-authenticated) list of IP addresses for bitcoin nodes.
    - The seeder obtains these addresses by periodically crawling the bitcoin network.
    - The bitcoin network has 6 seeders which are queried in 2 cases only.
      1. a new node joins the network for the first time
         - Query, if fail, then use a hardcoded list of 600 IP addresses
      2. an existing node restarts and reconnects to new peers
         - only if 11 seconds have elapsed but the node has less than 2 outgoing connections
  - ADDR messages
    - containing up to 1000 IP address and their timestamps
    - obtain network information from peers
    - accept unsolicited ADDR messages **only** upon establishing a outgoing connection with a peer
    - <= 3 ADDR message each containing up to 1000 addresses randomly selected from its tables once
    - 2 cases
      1. Each day, a node sends its own IP address in a ADDR message to each peer
      2. When a node receives an ADDR message with no more than 10 addresses, it forwards the ADDR message to two randomly-selected connected peers.
- Storing Network Info
  - Public IPs are stored in a node’s *tried* and *new* tables.  
  - Tables are stored on disk and persist when a node restarts.
  - *Tried* table
    - A *tried* table has 64 buckets
    - each bucket can store up to 64 unique addresses for peers
    - All stored peers have successfully established an incoming or outgoing connection
  - *New* table
    - A *new* table has 256 buckets
    - each bucket can store up to 64 unique addresses for peers
    - All stored peers have not yet initiated a successful connection

## Eclipse Attack

- Populating *tried* and *new*
  - exploits
    - the *bitcoin eviction* discipline
      - fresher addresses are likely to evict any older addresses
    - A node accepts unsolicited ADDR messages
    - Nodes only rarely solicit network information from peers and DNS seeders
- Restarting the victim
  - Try everything to make it restart (by cheating, DDoS,...)
- Selecting outgoing connections
  - After restarting, the victim makes all its outgoing connections to attacker addresses
- Monopolizing the eclipsed victim

## Countermeasures

- Deterministic random eviction
- Random selection
- Test before evict
- Feeler Connections
  - Add an outgoing connection that establish short-lived test connections to randomly- selected addresses in *new*
- Anchor connections
  - add two connections that persist between restarts

Raise bar of attacks
- More buckets
- More outgoing connections
- Ban unsolicited ADDR messages
- Diversify incoming connections
- Anomaly detection

# Relevance

- The P2P network security is a critical and low-level research topic in Blockchain 

# To discuss/investigate

- How to 'measure' the P2P network
- The way of simulating the attack needs more conern
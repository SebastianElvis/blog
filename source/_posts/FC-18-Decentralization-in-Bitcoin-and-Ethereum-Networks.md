---
title: FC'18 - Decentralization in Bitcoin and Ethereum Networks
date: 2018-12-13 19:10:10
categories:
- paper reading
tags:
- Decentralization
- Measurement
---




# Info

- **Title**: Decentralization in Bitcoin and Ethereum Networks
- **Conference**: FC
- **Year**: 2018
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://fc18.ifca.ai/preproceedings/75.pdf
- **Source code**: No

# Addressed Problems

- Few measurement studies on the level of decentralization of Public blockchains
- Current debates and decisiions about the underlying networks are often based on assumptions rather than measurement.

# Solution

- We present a comprehensive measurement study on decentralization metrics in these operational systems and shed light on whether or not existing assumptions are satisfied in practice.
    - We adapt prior Internet measurement techniques for Bitcoin and Ethereum and use novel approaches to obtain application layer data.
    - Data source:
      - direct measurements of these networks from multiple vantage points
      - a Bitcoin relay network called *Falcon* that we deployed and operated for a year
      - blockchain histories of Bitcoin and Ethereum
- Key findings:
  - the Bitcoin network can increase the bandwidth requirements for nodes by a factor of 1.7 and keep the same level of decentralization as 2016
  - the Bitcoin network is geographically more clustered than Ethereum, with many nodes likely residing in datacenters
  - Ethereum has lower mining power utilization than Bitcoin and would benefit from a relay network
  - small miners experience more volatility in block rewards in Bitcoin than Ethereum


# Methodology

## Bitcoin and Ethereum

- Bitcoin
  - P2P connection is based on TCP, by a 3-way handshake
    - exchange state like block height and version
  - When discover a new block, broadcast to the whole network and its hash (BlockHash)
  - Neighbor querys the block by BlockHash
  - Many block formats, like compact and Merkle
- Ethereum
  - GHOST protocol
  - Uncle blocks has rewards
  - Block interval 10-20 seconds
  - Block size determined indirectly by *gas*
  - UDP-based node discovery inspired by *Kademlia*
  - P2P connection is over TCP
  - P2P msgs are encrypted and authenticated, unlike Bitcoin

## Measurement Infra

- *Blockchain Measurement System(BMS)* - Falcon
  - a measurement system than ran experiments of varying duration–from a few days up to 12 months
  - uses multiple **vantage points** in order to gain a com- prehensive view of the cryptocurrency networks
  - continuously collecting data regarding the pro- visioned bandwidth of peers and peer-to-peer latency
  - first connects to a peer, collects measurements, and then disconnects before proceeding to the next peer
  - Targets:
    - Bitcoin nodes over IPv4, IPv6 and Tor
    - Ethereum nodes over IPv4 
      - ETH does not have Tor nodes as of May 2017, as Tor is exclusively TCP but ETH node discovery is UDP-based
      - No ETH IPv6 nodes because BMS was unable to discover enough nodes to reach generalized conclusions
  - Vantage points:
    - 15 out of 18 nodes reside in PlanetLab’s global research network [14]
    - the remaining three nodes are part of Cornell’s academic network, located in Ithaca, NY
  - Bandwidth measurement:
    - requires nodes
      - high download capacities
      - sufficient disk capacities
  - Node sampling
  -  BMS uses a list containing nodes from Bitcoin and Ethereum node crawling sites
  -  a locally deployed Ethereum supernode configured with a high peer limit

## Measurement

- Provisioned Bandwidth
  - by requesting a large amount of data from each peer and seeing how fast the peers can stream the data to BMS’s measurement nodes
    1. asking for blocks that were first seen over a year ago – similar to how a stale node asks for blocks to sync state
    2. BMS divides the time into epochs and records the number of bytes received during each epoch
    3. BMS processes the collected data to determine the provisioned bandwidth
  - Results
    - Bitcoin network has improved tremendously in terms of its provisioned bandwidth
    - experimental limitations and expected errors
      - the network bottleneck lies on the side of the measurement beacon rather than the remote peer
      - network traffic on the side of BMS interferes with the collected results
      - the remote peer intentionally shapes the traffic to selectively limit the bandwidth available to BMS
      - different steady state bandwidth between Bitcoin and Ethereum
- Network structure
  - we use the state of the art estimation techniques to establish bounds and gain insights into network structure
  - Single Beacon Latency
    - We first collect direct ICMP ping measurements from BMS nodes to all peers in the network.
  - Peer-to-Peer Latency
    - Measuring the peer-to-peer latency requires access to the end points.
    - we establish bounds from observed latencies from multiple beacons, using techniques from prior literature [37]
  - BMS also uses IP geolocation data to calculate distances between peer nodes as an additional validation on our results.
- Mining power distribution
  - we examined their weekly distribution over the last 10 months starting on July 15, 2016.
- Mining power utilization
  - measures the fraction of mined blocks that remain in the main chain
  - Bitcoin: >99%
  - Ethereum: ~95%. Drop when DAO event 

# Relevance

- This is a good reference for decentralization research
- Network measurement is critical for P2P systems

# To discuss/investigate

- Do something like Tor metrics or similar stuff?
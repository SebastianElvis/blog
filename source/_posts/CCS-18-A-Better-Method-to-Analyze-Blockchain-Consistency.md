---
title: CCS'18 - A Better Method to Analyze Blockchain Consistency
date: 2018-11-09 18:37:40
categories:
- paper reading
tags:
- CCS
- Blockchain
- Consensus
- Consistency
---


# Info

- **Title**: A Better Method to Analyze Blockchain Consistency
- **Conference**: ACM CCS
- **Year**: 2018
- **Topic**: Cryptography and Security (cs.CR)
- **URL**: https://dl.acm.org/citation.cfm?id=3243814
- **Source code**: No


# Addressed Problems

- Nakamoto's analysis on the Bitcoin consistency does not consider other attacks, and thus does not fully establish the consensus property for the protocol. 
    - For example, the analysis does not consider an adversary that attempts to introduce small disagreements between honest miners so as to split their computational power among several forks.
- State-of-the-art attempts of modelling Nakamoto consensus are with strong assumptions.
    - Garay, Kiayas and Leonardos [7] provided the first formal modeling ofNakamoto consensus and proved that the protocol achieved a common prefix-property. **However**, their analysis **only** considered a **static setting** in which the participants operate in a **synchronous communication network** in the presence of an adversary that controls a subset ofthe players.
    - To address issues in [7], Pass, Seeman, and Shelat [17] provide a different formal model for studying the properties of Nakamoto’s protocol. In particular, they introduce an **idealized model** for the protocol execution that can capture adaptive corruptions (and uncorruptions) of parties and a **partially synchronous network** in which the adversary can adaptively and individually delay messages up to some delay limit ∆.
    - In subsequent work in 2017, Garay, Kiayas and Leonardos [8] studied aspects of how the hardness p is adjusted as more players join the protocol during epochs in the Nakamoto consensus protocol and how this epoch must be sufficiently large to avoid certain attacks. Techniques from this paper were also used to update [7]; in particular, the updated version of the latter extends the analysis to the partially synchronous model **but is not precise enough to make concrete claims about the parameters.**


# Solution

- Our new analysis provides **a tighter guarantee** on the consistency property of Nakamoto’s protocol, including for parameter regimes which [17] could not consider.
- We analyze **a family of delaying attacks** first presented in [17], and **extend them to other protocols**.
- We analyze **how long a participant should wait** before considering a high-value transaction **confirmed**.
- We analyze the consistency of **CliqueChain**, a variation of the Chainweb [14] system.
- We provide the first **rigorous consistency analysis of GHOST** [20] and also analyze a folklore “balancing"-attack.

# Methodology

- **Section 2** formalized blockchain protocols introduced according to Garay, Kiayas and Leonardos [7] and Pass, Seeman, and shelat [17]: A blockchain protocol is executed in a partially asynchronous network model that involves the following components: (We use κ to denote the security parameter)
    - Environment
    - Honestplayers
    - Adversary
    - Random Oracle
- **Section 3** presented a **simple analysis framework** for blockchains that combines the approach of Pass et al. with natural *Markov chains* that capture protocol dynamics in the presence of adversaries.
    - Each state of our *Markov chain* represents an initial state of the blockchain system or the state of the system following an event of interest. Events include:
        - a new block mined by an honest player
        - a new block mined by the adversary
        - a sufficiently long quiet period
    - All of the Markov chains used in our analysis satisfy the following properties:
        - *time-homogeneous*: the probability of transitioning from one state to another is **only** dependent on the states, and **not on the time** at which the transition occurs
        - *irreducible*: it is possible to get to any state from any other state
        - *pergodic*: every state is periodic and has a positive mean recurrence time.
- **Section 4** analyzed the Nakamoto protocol using our Markov framework.
    - In §4.1, We **reviewed Pass et al.’s analysis** of Nakamoto using bounds on chain growth, block expiry, and the important notion of convergence opportunities they introduce for establishing consistency.
    - In §4.2, we reconsider **analysis of convergence opportunities[17]** using our Markov framework, and show how their analysis yields an underestimate.
    - In §4.3, we present **a new lower bound for achieving consistency** in Nakamoto’s protocol by an improved analysis of convergence opportunities using our Markov chain.
    - In §4.4, we present **a detailed analysis** of Nakamoto’s protocol under a **consensus attack**, deriving bounds on the probability that the attack can force forks of a specific length.
- **Section 5** **CliqueChain analysis**. In this section we explore another class of DAG protocols inspired by Chainweb [14]. In Chainweb, the blockchain is a block-braid made of multiple parallel chains in which each block must refer to blocks in specific braids according to a reference base graph.
- **Section 6** **Ghost analysis**. In this section we extend our method to analyze the GHOST protocol by Sompolinsky et al. [20]. 

# Relevance

This paper is a good reference for analyzing the PoW consensus process, especially the finality. The Markov chain model is a good tool to model the probablistic distributed consensus process like PoW.

# To discuss/investigate

Could we tune existing consensus protocols with the aid of this analysis?

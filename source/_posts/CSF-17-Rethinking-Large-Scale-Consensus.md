---
title: CSF'17 - Rethinking Large-Scale Consensus
date: 2018-12-10 12:17:40
categories:
- paper reading
tags:
- Consensus
- Public Blockchains
---

# Info

- **Title**: Rethinking Large-Scale Consensus
- **Conference**: IEEE CSF
- **Year**: 2017
- **URL**: https://eprint.iacr.org/2018/302.pdf
- **Source code**: No

# Addressed Problems

The question below hasn't been answered yet:
- Is consensus in the permissionless setting inherently mode "difficult" than consensus in the permissioned setting, and if so, what properties of the permissionless model introduce these difficulties?

# Solution

- We prove several simple but hopefully insightful lower bounds that demonstrate exactly why reaching consensus in a permissionless setting is fundamentally more difficult than the classical, permissioned setting.
- We then present a simplified proof of Nakamoto’s blockchain which we recommend for pedagogical purposes.
- Finally, we survey recent results including how to avoid well-known painpoints in permissionless consensus, and how to apply core ideas behind blockchains to solve consensus in the classical, permissioned setting and meanwhile achieve new properties that are not attained by classical approaches.


# Methodology

1. Proofs-of-work are needed without authentication
2. Proofs-of-work must be performed infinitely often in the presence of late spawning.
3. Honest-majority of computing power is needed in the presence of late spawning.
4. Protocol must know an upper bound of the network’s delay if uncertain of the number of players.

# Relevance

- This paper shows that permissionless Blockchains require PoW, which is highly relevant with design

# To discuss/investigate

- How should we "verify" this proof?
- The survey of this paper is interesting.
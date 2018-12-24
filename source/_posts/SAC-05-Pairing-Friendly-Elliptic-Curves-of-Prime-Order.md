---
title: SAC'05 - Pairing-Friendly Elliptic Curves of Prime Order
date: 2018-12-24 12:44:53
categories:
- paper reading
tags:
- Elliptic Curve
- Barreto-Naehrig Curve
- Pairing
---


# Info

- **Title**: Pairing-Friendly Elliptic Curves of Prime Order
- **Conference**: SAC
- **Year**: 2005
- **URL**: https://eprint.iacr.org/2005/133.pdf
- **Source code**: No

# Addressed Problems

- In spite of all these efforts, constructing pairing-friendly curves with prime
order has remained an elusive open problem since it was posed by Boneh et
al. [5, section 3.5] (see also [6, section 4.5]).

# Solution

- We propose a surprisingly simple algorithm to construct curves of prime order and embedding degree $k = 12$
  - The resulting security enhancement is even better than the lower bound of $k = 10$ required by Boneh et al.
  - Using the proposed method, even a 160-bit signature maps to 1920-bit field, where the best algorithms to compute discrete logarithms are worse than Pollard-rho in the elliptic group itself
- We next discuss how to compress the representations of points and pairing values to $\frac{1}{6}$ of their expected length for the proposed curves in section 3.
- We show in section 4 that the ability to handle large complex multiplication (CM) discriminants may have a positive influence on the minimization of $\rho$. 

# Methodology



# Relevance

- Pairing is a strong primitive for providing multiplicative homomorphism in Elliptic Curve Cyclic Groups

# To discuss/investigate

- BN Curve is widely used in Blockchain, so it's worth doing some research on it.
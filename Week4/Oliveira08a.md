# Note For Oliveira08a

## NABC Overview
* Need: Reveal the different between real AS-level Internet topology(ground truth) and the public view inferred by system log, BGP data.
* Approach: Using all kinds of data to inffer the Internet topology and compare with the ground truth in different tiers of ASes.
* Benefit: Finding that the inferred maps have a huge percentages error compare with the ground trup and argue a new approach to generate Internet map.
* Challenge: Inferring Networking topology have fundamental limitation and the lack of monitor in low level AS(stub networks) will make it impossible to infer all connections in AS-level.

## Key Ideas
* Compare with other paper:
	* this paper talks about completeness of prior AS graphs(how much data is missing)
	* validation the prior AS-level graph
	* actual ground ttruth is usually propritary
* are there any patterns in what is missing?
	* peering links can be hard to find 
* Data Sources:
	* BGP: has AS paths, public information, could see hijacking attempts
	* traceroute
	* IRR: official, might be out of date and incompletely
	* Router configuration files: operational, can have old, stale information

## Background Information(Section 2)
1. Relationship ship between inter-domain connectivits
2. No-valley-and-prefer-customer policy
3. Definition of **ground true**: Complete set of AS links
4. **Hidden Links** in network map: a connection has not yet been observed but will be revealed in long time period
5. **Invisible Links**: a connection will never be revealed due to no-valley policy
6. Several open source **data set** about Internet topology

## Case Study in different tier of AS

### Tier1 Network
1. Tier-1 AS's links are covered completely by public view
2. Tier-1 AS's links are covered completely by a single peer
3. Tier-1 AS's links are covered completely by a single customer
4. Other important thing:
	* Method to remove some outdated connection information

### Tier2 Network
1. Tier-2 AS's links are **not** covered completely by a single peer, all the missing links are peer-peer links
2. Tier-2 AS's links are covered completely by a single customer
3. Tier-2 AS's links are covered completely by public view, monitor by its customer or customer's customer, and the missing links are prefixes longer than /24

### Abilene and Geant
1. These two are academic used network in US and EURO
2. The public view is almost the same as the ground true
3. The different is from the passive monitoring session and IPV6 session(the paper only focus on IPV4)

### Content Provider
1. Content Provider's most links are public peer-peer link in IXPs
2. These links are almost **Invisible Links** so the public view is more than 85% miss, even include the IRR data

### Stub Network
1. The edge network of Internet, its degree is very small
2. The public view is almost the same as the ground true if the time period is long enough
3. The problem is almost no stub network have its monitor

## Conclusion
1. Tier1 network connection can be well inferred
2. Customer-Provider links can be well inferred
3. Peer-peer links can be high potentially misses in public view
4. The author argue a new heuristic approach to build the Internet maps by checking if two ASes both have routers in the same IXP(co-location) in another paper.


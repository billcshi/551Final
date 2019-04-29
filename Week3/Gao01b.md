# Note For Gao01b

## Overview
1. overview of interdomain routing
2. different AS relationship:
3. BGP Exporting potocol for each kind of relationship
4. Routing table entry patterns
5. An algorithm for inferring AS relationship
6. Result of the algorithm and verification

## Interdomain Routing Overview:
1. Connectivity does one imply reachability
	* Physical connectivity at the IXP does not necessarily imply that every pair of ASs exchanges traffic with each other
2. Several Tiers of ISP, the relation between ISP is different due to its tier
3. The degree of an AS can be a good heuristic in determining the size(tier) of the AS
4. BGP policies:
	* import policies
	* export policies
	* loop-avoidance rule
	* AS prepending: appends its AS number twice before export

## AS Relationships, export policies and entry patterns
1. 3 Relationship:
	* customer-provider(directed)
	* peering(undirected)
	* sibling(undirected)
2. Export Policies:
	* To Provider and Peer: its routes and its customer routes
	* To Customer and Sibling: its routes and its customer routes, its provider or peer routes
	* Peer and Sibling is symmetric but Provider-Customer isn't.
3. Entry patterns:
	* All Entry should be Vally-Free(Using a customer to transit datagram)
	* Downhill Path: A path only include provider-to-customer and sibling-to-sibling edge
	* Uphill Path: A path only include customer-to-provider and sibling-to-sibling edge
	* An Entry should be:
		* A Maximal uphill path and the maximal downhill path
		* A Maximal uphill path, a peer-to-peer path and the maximal downhill path
	* The different between sibling path and peer-to-peer path;
		* The peer to peer path only exist in top level transit

## Algorithm to infer relationship
1. Find the top level AS: the AS have largest degree
2. Distinct provider-customer and sibling-sibling relationship:
	* For Connected Pairs(u,v), if u provide transit to v and v provide transit to u, then they are sibling-sibling relationship otherwise they are provider-customer relationship
	* A better edition consider misconfigured of router:
		* count the number of entry that conclude transit relationship
		* only use the relation that appear more than L time, L is a constant number need to be fine-tuned
3. Distinct peer-to-peer relationship
	* According peer-to-peer only happen in top level AS, delete all relationship that isn't top level
	* A better edition: Delete the relationship which one AS has far less degree than the other

## Verification and Conclusion
1. Many kinds of misconfiguration
	* Router Configuration Typo: Loop existed in Entry
	* Misconfiguration of small ISPs: provide its provider to another provider, so it may be the transit between them
	* Unusual AS relationship
	* Inaccuracy of the heuristic: some top level AS have relatively small degree
#Note For Caesar05a

## Overview
1. Important process in BGP
	1. Exchanging Routing State Process
	2. Decision process in BGP
	3. Advertisement process in BGP
2. Common policies using BGP:
	* local policy
	* bussiness
	* traffic balance
	* scalability
	* security
3. Some more policy in developping
4. key ideas:
	* evaluation of routing between ISPs:
		* between ASes(AS: routers that share the same BGP routing policy)
	* BGP protocol
		* the EGP we used
	* Whay can BGP do for us?
		* cost effective
		* shortest path
		* flexibility for traffic engineering and business relationship
	* We care about the BGP mechanism(route attributes)

## Important process
### Exchanging Routing State Process(Overview)
1. overview:
	1. Border routers exchange routes between neighboring ASes
	2. Border routers distribute routes to internal routers
2. BGP salient feature:
	* incremental protocol
	* path vector protocol
	* advertised at the prefix level
	* messages contain several fields

### Decision process in BGP
1. Absence of policy: minimum path length
2. Policy: an order list of attributes
	* Attributes classification by source:
		* Locally: LocalPerf, IGP(Internal GP) cost
		* Neighbor: AS path length, MED
		* Neither(some is fixed): origin type, eBGP-learned over iBGP-learned, router ID
	* The order of attributes allows the operator to influence the decision process

### Advertisement process in BGP
1. 3 steps to deal advertisements:
	* import policy
	* decision process
	* export policy

## Common policy for BGP

### Some Terms
1. AS
2. IXP
3. RIB

### What is include in BGP UPDATE message:
1. prefix beging routed
2. with attributees:
	* AS-path
	* MED,LocolPref, etc.

### Local policy for BGP:
1. Preference(with attributes list)
2. Filtering: both before(inbound) and after(outbound) preference
3. Tagging: using community attribute(defind and use only by ISP itself)

### How business influence BGP policy:
1. 3 types of relationship:
	* peer-to-peer (usually without money)
	* provider-to-customer (with money)
	* backup
2. In decision process: Set different LocalPref for different relationship so it can make more money
3. In export policy:
	* Use community attribute to mark different relation and export different tables to different relationships
	* Usually need to delete the community attribute when export

### Traffic Engineering(Load balance):
1. Outbound control:
	* early exit routing
	* reduce congestion on outbound links to neighbors(By adjusting LocalPref, alway need manually operate)
2. Inbound control:
	* Goal: Local ISP to influence route selection in remote ISPs(By changing MED)
	* Prepend multiple copies of AS number to artificially increase the path length
3. Remote control:
	* Sharing community attributes(high risk if these community attributes have different meaning in different AS)

### Scalability
1. Limiting Routing Table Size(For limited memory)
2. Limit the Number of Routing Change(By invoking flap damping, add penalty when routing is changing)

### Security
1. Discarding Invalid Routes(By sanity checks in import policy)
2. Protecting Integrity of Routing Policy(Delete some attributes in import policy)
3. Securing the Network Infrastructure(Filtering some important IP in export policy)
4. Blocking DOS Attack(Using black hole router)

## Developping policy
1. Configuration Checking
2. Language Design(RPSL)
3. New architectures

## Internal a ISP
1. Why OSPF?
	* OSPF react quickly(internal network changes)
	* OSPF reroutes around changes quickly
2. Why iBGP(Interal BGP between routers in AS)
	* propagate policies across out network

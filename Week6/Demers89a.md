# Note for Demers89a

## NABC Overview
* Need: The gateway(router) need to provide fair allocation of throughput and delay to all of its users, even there are **some ill-behaved user**
	* **Fair Allocation** Based on** max-min fairness criterion**: (in Page 3)
		1. no user receives more than its request
		2. no other allocation scheme satisfying condition 1 has a higher minimum allocation
		3. codition 2 remains recursively true as we remove the minimal user and reduce the total resource accordingly
	* **user** means **source-destination pairs** in network
* Approach: A fair queueing algorithm runing on gateway(router) and also simulate and compare with former queueing algorithm(FCFS)
	* The flaw in fromer papers[Nagle95&Nagle97] is lack of consideration of packet lengths
	* This paper's algorithm is implement a packet-by-packet algorithm to emulate the bit-by-bit algorithm(each users share **the same amount of throughput in bit-level**)
	* This algorithm also punished the ill-behavior in the network
	* In comparison part:
		* This paper concerned different kind of congest avoidance algorithm run on source mechine
		* It also concerned if the network existed ill-behavior
* Benefit: The Fair-Queueing(FQ) algorithm significantly increase the fairness in network and decrease the delay for small packet source, who use lower throughput than its fair share
	* This algorithm also punished the ill-behavior which can be saw in *Scenario 3*, which may lead to network fault in all FCFS queueing algorithm
	* In almost all test cases from the paper, FQ have better proformance in the delay of small-packet source
	* The network is fair even it a highly congestion network, but not totally fair due to the generic TCP congestion avoidance algorithm's flaws
* Challenge: The fair doesn't mean equal to every user and it should provide some user higher throughput and the technology to product such an router is difficult at that time

## Key Ideas
1. interaction between queueing at routers and congestion
	* maybe routers can indirectly prevent congestion
	* FQ can control how different flows mix
	* different from TCP fairness control:
		* TCP is in the end host and FQ in the router
		* **FQ enforce fairness**, which is very powerful
2. methodology: start with impossible but understandable model, then convert to realizable but harder model
3. Fair in conversation(aka flow: src-dest addr-port pairs) level

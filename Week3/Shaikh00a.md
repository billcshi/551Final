# Note For Shaikh00a

## Overview
1. Background: former paper about congested network
	* Most of the studies assume reliable and loss-free delivery of routing control message
	* Losses of the routing protocol message lead to route flaps and instabilities
	* This paper is first attemp to analyze the dynamics of routing protocols
2. Build an analytical model of congest network
3. Analyze the model using markov chain
4. Experimental results and compare with section 3
5. Summary
6. Key Idea:
	* Evaluate OSPF and BGP under congestion
		* can fail unexpectedly under congestion
			* problem: control traffic no isolated from data traffic
		* two different protocols used in different places
		* they use different transport protocol: TCP and (UDP)
	* method: 
		* emulate the network in testbeds(for experiments)
		* model the failure and recovery of the protocols
		* compare the results from models and the testbeds

## Model Setup
1. Network model:
	* 2-nodes model(source router to receiver router)
		* after failure, traffic goes over same link
	* 3-nodes model(contain 2 paths,a two-hop path(shorter path) and a three-hop path(secondary path))
		* after failure, traffic goes over the other link
	* congested **only** in **the source router's output**
2. Variable:
	* Independent variable:
		* **Overload Factor**
		* Buffer memory(only in 3-nodes model)
		* Buffer drop protocol(Drop-from-Front and Drop-tail, relative to **queueing delay**)
		* **Router Protocol: OSPF and BGP(TCP)**
		* ~~Package Size~~(Final it prove to be **Unrelated** variables)
	* Control variable:
		* Queueeing and Scheduling: FIFO
	* Result:
		* U2D time, the time it takes for a routing flap
		* D2U time, the time it takes for recovering

## Analyze the model
1. Assumptions:
	* The overload factor remains constant
	* Every packet has the same probability p of being dropped

### OSPF model
1. How to occur a route failure(~~flap~~):
	* source will provide a hello mesage in TH1 (HelloInterval,10s in paper and plus 1s as error rate)
	* receiver will acknowledge hello message using a timer in TRD1(RouterDeadInterval, 40s in paper without error rate)
	* if the interval between 2 hello message more than TRD1 then the rout flap happen
	* note that OSPF control messages are sending using IP datagrams directly
2. Using a 5 states Markov chain(Figure 2) to describe the U2D cycle
3. The Eq. (1) show the expected flap time in this Markov chain
4. How to recover:
	* receive a hello message
5. D2U time is easy to calculate in OSPF protocol, it is **TH1/(1-p)**

### BGP model
1. It's **hard** to deriving an accurate model for BGP, because BGP use TCP to transmit and retransmit the routing message
	* TCP is adaptive about the retransmit time
	* the number of states of Markov clain is equal to the time of retransmit
2. Figure 3 show the Markov chain for U2D cycle of BGP and Eq (4) show the expected flap time based on that:
	* the variable n is based on RTO and RTT in TCP protocol, which is overestimating in the paper
3. The D2U model in BGP need to concerned the direction from client to service or opposite:
	* It need to build the TCP first and then the BGP
	* Building TCP need 2 message sending from client to service and 1 in opposite direction
	* The Markov chain is completely different in different direction, Figure 5 and 6 show the different chains
4. The U2D situation:
	* send KEEPALIVE message
		* every 60s
	* have keepalive timer if it expires declare link down
		* timeout in 180s	

## Experimental Results
1. Packet size is unrelated
2. Buffer size is unrelated to 2 nodes model
3. RTT is overestimate so the flap time is overestimate
4. BGP takes longer to fail and OSPF takes faster to recover:
	* BGP retry faster due to RTT which is << 10s
	* however TCP need to setup


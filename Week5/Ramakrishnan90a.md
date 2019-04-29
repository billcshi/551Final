# Note for Ramakrishnan90a
## NABC Overview
* Need: Network congestion need to be control and the user should be fair
* Approach: Policy implemented in both router and user
	* Router: Set the congestion bit in packet header
	* User: Copy the congestion bit to Ack packet and adjust the window based on congestion bit and policy
* Benefit: The network is more power(higher utilization) and fairness and this algorithm doesn't need to send additional packet
* Challenge: Each user in the network should obey the policy, otherwise this policy will lead to truly unfair
* Key ideas:
	* for connectionless network(like IP)
	* using a binary feedback scheme(and packet loss) and hopefully *avoid* congestion and not just go to packet loss
	* this paper carefully lines up each design option

## Policy Detail
1. Different between congestion control and congestion avoidance:
	* Congestion Control: Let network never go to cliff
	* Congestion Avoidance: Let network on the knee point of throughput-response time(delay)
2. Former Research: Need to send additional packet when the network is already congest

### Overview
1. For Router
	* Congestion Detection: Based on average queue length
	* Feedback filter: deal with network jittera(avoid oscillation)
2. For User
	* Decision frequency
	* Use of receive information: Whether maintain the information after decision
	* Signal filtering: deal with network jitter
	* Decision function: Additive increase and multiplicative decrease

### Optimization Criteria
1. Power:
	* define: Throughput ^ a / Response time
	* if a=1, then power is maximized is the knee of the delay curve
2. Efficient:
	* define: resource power / resource power at knee
3. Fairness: which is fairness index still used today
	* define: square of sum / n * sum of square

### Router Part
1.  Using simple thresholding policy or hysteresis policy:
	* The paramater for thresholding
	* The paramater for hysteresis
	* set constant value by experiement
2. Using instantaneous queue size or average queue size:
	* 2 disadvantage for instantaneous queue size:
		* delay for feedback
		* result in unfair since different user receive different signals
	* The paramater for average interval: using adaptive algorithm
		* From last busy regeneration point to now

### User Part
1. instant decision when ACK or use a relatively long period data:
	* instant decision will lead to prematurely alters the window size and causing overcorrection
	* The time period set to Wp+Wc:
		* Wp: is the former windows size, for ensure all preview package is ACK
		* Wc: is now windows size, which is used to make decision
2. How many ACK packet used to decision: Wc
3. How to deal with jitter:
	* use single threshold for average rate of congestion bit
4. How to alter window size:
	* Why not additive increase and decrease:
		* unfairness, the gap between 2 user will preserved due to add or subtraction the same number
	* Choose additive increase and multiplicative decrease:
		* If only interage number used, still lead to unfairness in special case
		* Use float for windows	

# Note For Quan13c

## NABC Overview
* Need: Find a method to monitor the network outage in global Internet.
* Approach: A method, called Trinocular, which uses active probes to detect the disconnected and inferred the outage by Bayesian inference.
* Benefit: This method have higher coverage rate and can affordable traffic.
* Challenge: The parameter in the model is fixed by historical data which can be adaptive by real time measurement

## Some Detail about Trinocular Method
### Responses for Ping
1. positive reply: some computer is live at the IP address you're pinging
2. err(negative reply)
	1. if you have an error in your ECHO REQUEST
	2. you get a HOST UNREACHABLE from the router
3. no reply
	1. no one's there
	2. firewall and they don't want to reply
	3. query or reply was dropped
	4. when there's a network failure (**We really care about this**)

### Simple Outage-Centric Bayesian Model of Internet
1. b denote each /24 block
2. E(b) denote the ever active addresses in block b
3. A(E(b)) denote the expected response rate of the address in E(b)
4. Note that:
	* E(b) and A(E(b)) are independence
	* the method only send probes to address in E(b)
5. For a block which is up(reachable):
	* the rate to receive a positive probe: A(E(b))
	* the rate to receive a negative probe: 1-A(E(b))
6. For a block which is down(unreachable):
	* the rate to receive a positive probe: (1-l)/|b|
	* the rate to receive a negative probe: 1-(1-l)/|b|
	* l, denote the combination of probe and reply loss
	* |b|, denote the block size
	* Receive a positive reply while a block is down, only happen if a single router is up but all addresses behind the router is down
7. Update the Inferred State based on the new probe reply by Bayesian inference:
	* B(U), denote the current belief of the block is up
	* B'(U), denote the next belief of the block is up
	* B(U'),denote the current belief of the block is down
	* When receive a positive reply:
		* B'(U)=B'(U|Possitive)=P(possive&U)/P(possitive)=(P(possitive|U)B(U))/((P(possitive|U)B(U))+(P(possitive|U')B(U')))
8. Multiple Observers:
	* distinguish between outages near the VP vs. near the target
	
### The Probe rate and Traffic Analyze
1. Periodic probing: every 11 minutes
2. Adaptive probing: 
	* If belief is more than 90% or less than 10%, we believe the state is determinded
	* If the state is uncertain, we need to probe more adaptive probe
	* Send new probe as soon as the last probe is resolved, until the we reach a conclusive belief
	* Maximum of 15 probes will be send, if it still uncertain then we mark this block uncertain
3. Recovery probing:
	* If the A(E(b)) is low in some block, the down-to-up recovery may be false negative due to some unoccupied address
	* We send 15 probe to decrease the false negative rate
4. total traffic: about 0.7% of total background package in the analyzable blocks
5. Coverage rate:
	* We delete the block with very low E(b) and very low A(E(b))
	* 24% of routed space is analyzable in this method
6. probe from multiple location:
	* parallelize the work
	* different locations can access different address

### Validation
1. The correctness: for block A(E(b))>0.3 and outage duration longer than 1 round will always be detect
2. The timing of duration is lated with a uniform distribution between 0 and 1 round
3. In reanalyzing historical Data:
	* Revealing several outages cause by human or nature

## Compare with other method
1. Control-plane is indirect and provide incomplete coverage of all outage types
2. Client-support Analysis allows better fault diagnosis but fewer coverage
3. Passive Data Analysis which is needed to mind "low-quality" data source.
4. Data-plane:
	* which address to choose:
		* .1 address and hitlist address, recall rate is relatively lower because some .1 address is unoccupied thus other address in the block is reachable
		* all address, the information per probe is too low and the traffic is unaffordable
	* Choosing blocks or prefiex:
		* using prefix have large coverage, but some prefixs are too sparse so prefix will overstates outages

# Note For Dhamdhere18a

## NABC Overview
* Need: Discussions of persistent interdomain congestion at IXP without direct access to them
* Approach: A third-party monitor system for interconnectiong congestion
* Benefit: This system is well inferring the congest in interdomain links and validation with several data
* Challenge: The router configuration, asmmetric routes will lead to wrong inference and the cause of congestion still unreveal in this paper

## Key Ideas:
1. probing remote inter-domain links
	* want to prove if there is persistent congestion
	* so we can fix the problem
2. goal: discover persistent congestion
3. mechanisms
4. validating the results

## Detail about inference and validation method
1. TSLP(Time-Series Latency Probing) Method:
	* sending ICMP probes to both the near and far ends of an identified interdomain link
	* measuring the latency between probe and reply
	* a possible cause of the increased latency is congestion
2. bdrmap Method:
	* bdrmap Method is used to infer the interdomain links between the host network and the neighbor networks
3. Level-shift Method:
	* the algorithm detects level-shifts of duration at least l/2.
	* l denotes cut-off length of time bin(5 minutes in paper)
4. Autocorrelation Method:
	* Find multi-day repetition of elevated delays at the same times of day
	* The time window in the paper is 50 days
5. Validation Method:
	* **Packet Loss** is used to validate the congest inferred by the system, used when at least one episode of congestion has been inferred by one of the two method
	* **Throughput** is provide by NDT, and used to validate the congest
	* **YouTube streaming** is using YouTube-Test Tool to measure ON-period throughput, Start-up delay and Streaming failure rate in congest and uncongest period.
	* Also validate by operator in ISP

## Conclusion
1. Several validation datas show the system can well inferred the congest in interdomain with monitor in edge
2. Congestion was not widespread on the peer.provider interdomain links
3. Some show significant congestion from AP to Content Provider

#Note For Padhye98a
## NABC Overview
* Need: Need to analyze the TCP throughput accurately.
* Approach: A computational model for TCP throughput based on loss rate, RTT, RTO and TCP protocols(Both fast retransmit mechanism and time-out mechanism).
* Benefits: The model provides a more accurate result for TCP throughput than former model over a wider range of loss rate.
* Challenge: Some part of TCP congestion control protocol is not considered, so the correctness of the network in the network with high congestion level needs to be confirmed.
* Key Ideas:
	* estimate steady-state throughput for bulk transfer TCP
	* easier to examine an equation to find behavior in different situation

## Other Information
1. Key Idea: Proposing and validating a model to predict TCP throughput based on TCP window-control protocol, likes triple-duplicate acks and time-outs.
2. Novelty: The former paper and model about throughput analyze only focused on TCP triple-duplicate acks protocol and this paper considered not only the former one but also the time-out protocol which happens more often and affected the total throughput.
3. The methodology the paper used is analysis and experiment. The author proposed the model by analyzing the protocol detail and validating the model by experiment in 37 TCP connection across the US and Europe.
4. The thing this paper can improve:
	* This paper only focuses on the part of the TCP protocols, however other protocols, likes slow-start, should be considered in the paper. The time-out event(TO) is relatively higher than triple-duplicate acks(TD) event. The increment of in experiments according to the paper. And the increment of TCP window size after recovering from time-out is much faster than the TD event. So consider slow-start in the model will improve the accuracy of the prediction.
5. Question 1: Last few weeks we talk about congestion in the network, what will happen in this model if this probability is high(p≈0.5)?
6. Question 2: The [Jacobson88a] paper talks about the slow-start protocol in TCP, what will happen if we consider this in the model.
7. Answer 1: 
	* When the probability of packet loss is high(p≈0.5), and setting b=1 in the protocol. I found something interested in this model:
	* The expected windows size before fail: E(W)=3
	* The probability that a loss indication ending a TDP is TO: Q≈1, which means almost all the failure is caused by time-out event if p is high
	* The average duration of a time-out sequence is: E[ZTO]=8T0
	* The prediction of throughput in this paper model is: B(p)=\frac{5}{2.5RTT\ +\ 8\ T0}
	* The prediction of throughput in only-TD model is: B(p)=\frac{4}{2.5RTT}, This result is much larger than the result of the article, because it misplaces the cause of the network error. 
8. The paper is important or not:
	* This paper is an important paper. It expresses the TCP window management protocol in a computable model and derives the model for calculating TCP throughput. This perdiction model requires only three variables that can be easily obtained. Secondly, this model is easy to expand. The article shows the process of extending from a model considering the only TD to a model considering TD and TO.
9. Relate to [Jacobson88a] which introduce the detail of TCP congestion control and avoidance protocol, and this paper is building the model based on TCP protocol(some detail methods are different between two papers).


# Note For Jacobson88a

## NABC Overview
* Need: Need to avoid and control congestion in Internet
* Approach: Implement several congestion control protocols in TCP protocol
	* slow-start protocol(soft start)
	* RTT mean estimator
	* RTT variation estimator
	* congestion avoidance protocol
* Benefit: Increasing the effective bandwidth in Internet
* Challenge: These approaches were implemented in hosts, other protocols need to implement in Gateway to make the Internet **fairer**

# Key ideas
1. principles: packet conservation, avoid congestion collapse
2. proposes algorithms to avoid and recover from congestion
3. better RTT computation

## TCP protocol detail in Paper
1. 3 ways for package conservation to fail:
	* The connection doesn't get to equilibrium
	* A sender injects a new packet before the old one has exited
	* The equilibrium can't be reached because of resource limits along the path

### Slow Start Protocal
1. Deal with the 1st Failure
2. Add 1 paramater and 3 lines of code to TCP protocol:
	* Add a congestion windows,** cwnd**, to the per-connection state
	* When TCP starting or restarting after a loss, set **cwnd=1**
	* When receive an ACK, increase cwnd by 1
	* When sending send the minimum of receiver's advertised window and cwnd
3. Which seem to be increased slowly(by 1 each time), but acctually it double every round(since you will receive an ACK for every package in the windows), want to get to steady state quickly
4. When to start: at connection start or when we restart after congestion
5. When to end: When we detect congestion OR when we reach steady state(cwnd\>ssthesh) (OR we reach the maximum **flow control window size**)
	* ssthread should approximate the bandwidth-delay products
	* ssthread initial value ... based on guess 64K(different constant values in different implementations)

### A cheap RTT estimator(in Appendix A)
1. Deal with the 2nd and 3rd Failure
2. A cheap method to estimate the RTT mean time and the variation we used these parameters:
	* g for mean: a filter gain constant for RTT mean with suggested value of 0.1~0.2
	* g for variation: a filter gain constant for RTT variation with relatively bigger value than g for mean
	* m: the measure time for transmit
	* a: the estimator for RTT mean time
	* Err: Err=m-a, the error between the real time and estimate time
	* v: the estimator for RTT variation time, Here **we use the mean deviation rather than standar deviation**
	* Err-v: the gain part for mean deviation
3. We use the **mean deviation** rather than standar deviation:
	* avoid multiple calculation
	* mdev = 1.25 sdev, in normally distributed
4. So we got the algorithm without scaling:
	* Err = m - a
	* a = a + g\*Err
	* v = v + g'\*(Err-v)
5. To avoid the float calculation, we need to keep everything integer:
	* g for mean = 0.125 = 1/8
	* g for variation = 0.25 = 1/4
	* Multiply the whole parameters in algorithm by 1/g(a=8\*real\_a and v=4\*real\_v)
5. Algorithm detail after scaling:
	* Err = m - (a >> 3)
	* a = a + Err
	* Err for v = Err - (v>>2)
	* v = v + Err
	* rto = (a>>3) + v

### Congestion Avoidance Protocol
1. Deal with 3rd Failure
2. Key Idea: 
	* increase the window sizes slowly:
		* If it increase in multiplicative speed will drive the net into saturation and hard to recover
3. Algorithm:
	* On any timeout set swnd to half and current window size
	* On each ACK increase cwnd by 1/cwnd
		* probing for unused capacity
		* The rate increase linearly
	* When sending send the minimum of receiver's advertised window and cwnd
4. The slow-start and congestion avoidance protocol seem to be confused, and they actually independent

### Slow-start and congestion avoidance protocol together
1. Key Idea:
	* Add a new paramater, ssthresh, to record the threshold size
	* Use slow-start to deal with the situation when the loss happen and need to recover
	* Use congestion avoidance to deal with the threshold and window increased after hitting the threshold
2. Recover:
	* ssthread = cwnd/2 , binary search is a good way to converge quickly on the true best rate
	* cwnd =1, give the network some time to clear its full buffers
3. If we want to avoid the pipe empty due to recovering:
	* Fast retransmit, duplicate ACK
	* duplicate ACk imply you missed packet n+1, but get some packet out of order 

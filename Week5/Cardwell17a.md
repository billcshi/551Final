# Note for [Cardwell17a]

## Key Ideas:
1. another congestion control algorithm: defining optimal operating poing
	* In R&J paper, their knee as optimal throughput/delay
	* this paper: range of behavior near knee
		* left side: no queue but full bitrate
		* right side: large queue (before drop) and full bitrate
	* below the knee: application limited
2. estimate: RTT, bottleneck bandwidth
3. from observation in the network
	* avoid packet loss(Not use it as an observation)
	* estimate data delivery rate EstBtlBw
	* effective Round Trip Time( = physical RTT + queueing delay)
4. why model the network now?
	* we think we can do better with different parameters
	* want to do better than waiting for packet loss
	* changes to network: MUCH faster and MUCH bigger buffers
	* end hosts are faster(In Clark88a paper we cannot have a timer for each TCP connection, which is too expensive)

## NABC Overview
* Need: Since the internet change in last 30 years, the loss-based TCP congestion control is not well perfromed. We need to alter the protocol to ensure the Internet running on both maximum throughput and minimum delay.
* Approach: BBR TCP implement, which is a congestion control protocol based on **Round-trip propagation time(RTprop) and Bottleneck bandwidth(BtlBw)**.
	* Algorithm designed to estimate RTprop and BtlBW
	* Maximum Inflight = BtlBW * RTprop
	* Several algorithm to control packet send to ensure the inflight traffic never exceed the maximum
* Benefit: Almost full bandwidth utilization and least delay, compared with CUBIC.
	* BBR does better with large RTT
		* can avoid congestion by not probing until loss
		* without congestion, you don't have to recovery
	* BBR does better with large bitrate
		* multiplicative increase gets to "large" must faster than additive
* Challenge: Still need all sender in a network obey the BBR protocol

## Detail about BBR algorithm

### Ack-arrival half
1. Used to estimate and update the RTprop and BtlBW:
	* RTprop = minimum(RTT), assume there is no queueing in the bottleneck
	* BtlBW = delivered\_packet\_size / now-delivered\_time

### Data Send half
1. Calculate Real-time bdp(Traffic in pipe)
2. Check if bdp exceed the inflight limitation
3. Check if the last packet already send and these sized of other packet already exited the network pipe
4. Send the packet
5. Set the next packet sending time, which is after this packet is completed sending

### Steady-state behavior
1. Realtime monitoring the network change
2. By sending probe in about 2% total time(200ms in every 10s)

### Start-up behavior
1. Just like other TCP implementation, it start from 1 packet and double every time
2. The different, after got the max size it need to drain the buffer cause by start-up stage

### Multiple BBR
1. Using probe and ProbeRTT(rather than RTProp) to realize fairness

# Note For Huang13a

## NABC Overview
* Need: The 4G LTE network provides high bandwidth and low delay network. These new characteristics don’t fit some of the TCP protocols, which let the utilization of 4G network relatively low(lower 50% in more than 70% cases)
  * The low utilization because of small windows limits(discussed in Padhye98a)
  * Big queues and buffer bloat in the network
  * 4G LTE wakeup to send IP packets which is not energy efficient(Delay FIN)
* Approach: Author captures the packet headers in Monitor and builds a testbed in his laboratory to measure the characteristics of the LTE network. And analyze the result to point out some unfitness between TCP and 4G LTE network.
  * This paper also state a bandwidth estimation algorithm.
* Benefit: This paper provide several suggestion in design the mobile APP using the LTE network:
  * Read the TCP buffer to the application layer buffer ASAP.
  * Using DUP-ACK to estimate the RTT and RTO
* Challenge: No experiment to convice that theese suggestion will improve the TCP and LTE network.


## Other Information
1. The structure of the 4G LTE network:
	* 3 subsystems: user equipment(UE), the radio access network(RAN) and the core network(CN)
	* Within CN: SGW & PGW(serving gateway and packet data network gateway), PEP and Firewall
	* About PEP(**The Performance Enhancing Proxy**): The HTTP packet(port 80 or port 8080) will traverses through the PEP. And PEP splits the end-to-end TCP connection into two, from UE to PEP and from PEP to target Servers.
2. Some characteristics of LTE network:
	* Heavy-tail distribution: Most of the packet is very small, but 0.6% packet(more than 1MB) account for 61.7% links.
	* Delay FIN(in Figure 4) which will lead to the waste of device energy. Because the LTE radio interface will auto shut down when the counter is out. But the delay FIN packet will reopen the radio interface.
	* LTE promotion delay, the radio interface will spend the time to turn on.
	* DNS lookup is very fast due to the DNS message is caching in its DNS servers in CN.
	* Queueing Delay: High queueing delay is building up in the TCP buffer.
	* Low transport-layer loss rate: Due to the retransmission in physical/MAC-layer.
3. Abnormal TCP Behavior:
	* Due to the high queueing delay in TCP buffer, the TCP quick retransmission protocol(Dup-ACK) will let the TCP window size very low and significantly influence the utilization of LTE network.
	* One solution is using Dup-ACK to update the RTT and RTO estimator.
	* The other solution is using a fall-back approach to estimate RTT.

## Different between Wireless and Mobile

1. wireless:
    * shared media
    * interference with other user
    * may have choice of multiple access point
    * hidden terminal problem
2. mobile:
	* have to handle hand-off from one base station to another
	* battery(energy) usage is matter
	* IP can change, especially for long-term link(e.g. file download)
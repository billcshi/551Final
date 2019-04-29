# note for Clark88a

## Overview
1. Capture some early reasons which shaped the Internet protocols:
	* the idea of datagram(In Section 9)
	* layers seperation for TCP and IP(In Section 5)
	* original objectives of the Internet architecture(In Section 2)
	* the relation between goals and feature(In Section 3-7)
2. Some Basic Idea of Internet:
	* Datagram(In Section 9)
	* TCP(In Section 10)
	* Flow(Future Building Block, In Section 11)

## Fundamental Goal of Internet
1. effective technique for multiplexed utilization of existing interconnected network
2. Why package switching? Why no other technique, likes circuit switching?
3. benefits of using circuit switch for phone:
	* each circuit is not interfered with:
		* you get your bits and fixed latency
		* won't get terminated
	* phone companies can engineer capacity
	* get cheap handsets
	* easy for accounting
4. Packet Switching(Internet):
	* something worse than circuit:
		* more overhead in each packet headers
		* packets can be out of order
		* queueing can delay you, can it can vary(**jitter**)
		* can be out of capacity(in the middle of an interaction)
	* advantage:
		* statistical multiplexing:
			* we let people in, interleave their traffic, depend on statistics and idle time to make sure we're under capacity
			* implications: interference and possible overcapacity, congestion management
5. The fundamental structure:
	* A packet switched communications(gateways) which implement a store and forward packet forwarding algorithm

## The Goal and related feature
7 goals which were established for Internet:
1. Survivability: 
	* What kinds of failures: host, link, routers, line noise
	* solution: fate-sharing approach, only check at the endpoint
	* pro: protect anainst any number of intermediate failure, easier to engineer(for network)
	* con: hard to engineer(for host), not cost effective for retransmission
	* other solution: checksums, redundancy
2. Multi-type of service:
	* Why?
		* different appilications need different services
		* different services have different requirments
			* quick startup
			* steady rate
			* real time
	* solution: IP and TCP layers seperation
	* use datagram to be the building block of services
3. Varieties of networks:
	* minimum set of assumptions:
		* reasonable size
		* reasonable reliability
		* suitable form of addressing
4. Other 4 Goals(Not that important)
	* Distributed management: by exchange routing tables
	* Effitive: Not solve but reasonable not that bad
	* Attaching of new host: Become harder because of the solution of survivabilitya
		* in 1980s: implement the protocols
		* 10 years ago: configuring addresses
		* today: authentication
	* Accountability: not that important in 1980s but important today

## Architecture and Implementation
1. The architecture tried hard not to constrain the range of service
2. Hard to guide the designer of realization of Internet:
	* Lack performance measure(Only on logic correct)
	* Lack Simulator
3. Whay is Architecture and Implementation:
	* Architecture: the components and protocols of the net(see the RFCs)
	* Implementation: consider real-world constaint(like memory, processor)
4. realization: an instance of the Internet class, different requirement for different class

## Datagram
1. Why datagram?
	* eliminate the need for connection state
	* a basic building block for all kinds of services
2. Using datagram **Does Not** equal to high level service should be datagram, usually it is more complex

## TCP
1. Some of the early reasoning for the parts of TCP:
	* flow control by bytes:
		* original ARPANET: flow control based on both bytes and packets
		* TCP only use bytes:
			* permit the insertion of control into sequence
			* permit TCP packet to broken up
			* permit gathered small packet together
		* However,
			* acknowledged the control as well as data make the idea complex in practice
			* packet borken was moved to IP, and this make hard for IP protocol
			* The small packet problem may not happen if control by packet.
		* Maybe the original ARPANET approach is better.
	* EOL(End-Of-Letter flag):
		* the data up to this point in the stream was one or more complete application-level elements
		* EOL should be a tool for mappint the byte stream to the buffer management of the host
2. Other protocols (than TCP/UDP/IP):
	1. ICMP:
		* why not TCP?  don't want retranmissions
		* want it to be simple
	2. QUIC:
		* TCP replacement running over UDP, originally from google

## Conclusion and Future Network
1. Resource Management(QoS) and Accountability are difficult to achieve in context of datagrams
2. A better building block for Internet:
	* flow
	* gateway should have flow state to remember which flow are passing through them
	* flow state information would not be critical with service
	* the service would be enforced by end point

## NABC
1. hook: what went into this "Internet" thing
2. need: understanding the Internet's history can help you improve it
3. approach: what are the design principles behind the Internet
4. benefits: when you build new things over the Internet, you can do them more efficiently
5. challenges:there were competitors to the Internet: ISO network stack, DECnet, ATM
6. close: whay should we do next to the Internet: resource management(QoS)

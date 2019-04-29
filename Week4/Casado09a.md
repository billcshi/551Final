# Note For Casado09a

## NABC Overview
* Need: How could we make enterprise network more manageable.
* Approach: Ethane architecture which enforce a single network policy by a central computer(Controller)
	* Binding a packet and its origin(user, host, port, Switchs port, etc.)
	* All decision is made by a centralized Controller(PC)
* Benefits: The architecure is more manageable than other in these fields:
	* Indentify network problem based on journaling registrations and bindings
	* Straightforward to add new features to the network
	* very few Controllers can scale to support a large network
	* Switches in this architecture is very simple
	* Ethane can be incrementally deployed without any host modification
* Challenge: Some shortcome due to Ethane is base on end-to-end argument in application layer
	* Broadcast and Service Discovery(ARP, OSPF, etc.) still constituted most flow
	* The Ethane is in application layer, so it cannot prevent from lower layer(likes IP)
	* Still miss knowledge about user, so it is hard to detect which application is using the port
	* Spoofing Ethernet address in a no-Ethane-only network
* Other contribution in this paper:
	* FSL(Flow-based Security Language) to describe network policy

## Architecture Design:
1. Naming and Binding
	* switch port to machine(MAC)
	* machine(MAC) to IP
	* User to machine(MAC)
	* must know who sends packets and when they start a new route
2. Policy Language(FSL)
	* high-level policy
3. Controller:
	* Register and Track each names and binding
	* Journal all bindings and flow-entries in log, for diagnose network fault
	* Bootstrapping: Create and Maintaining a shortest path tree for switches
	* Authentication: allow or deny user's action in network
	* Setup Flow in the network: add new flow entry and policy to switch(only the first packet need to send from switch to Controller, and the subsequence can use the established entry)
4. Switch:
	* Much simple than origin switch
	* Switches setup: Send register message to Controller
	* Forwarding a packet:
		* If it has a entry, forward or deny as the entry
		* If it has not a entry, send to the Controller and add a new entry
		* The entry is per flow and per switch
		* The entry will be cancel when time is out or revoked by Controller
	* recover from failure:
		* contect the control for everything like new flows to rebuild the flow table
5. Replicating the Controller for Fault-Tolerance and Scalability
6. Recover the network from link failures
7. FSL language:
	* A lookup table of rules
	* Each entry consisting of a condition and a action
	* condition include the predicate function and arguments, also include boolean predicate
	* action include standard set and also C++ or Python function
	* 2 ways to deal with rules conflicts:
		* priorities ordering
		* apply least restrictive action
8. Concerns about a Central Controller
	* performance
	* what happens if it fails? backup-controller, revert back to a distributed netwrok, recover from cold-start all the flow
	* who has access to the controller: change policy, DOS attack, the metadata of network

## Compare to [Clark88a]
1. In 1988 the accountability was the bottom but in 2007 big focus on accountability
2. In Clark's paper show decentralized control, in this paper provide centralized control inside enterprises
3. In 1988 each user trust other, but now it loss these trust

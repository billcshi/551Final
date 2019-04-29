#Note For Saltzer82a

## Overview
1. A perspective on the subject of names in network.
2. Distinguish 4 kinds of destination
3. Use the binding(in Operating System Concept) to describe the relation among 4 destinations:
4. Real-World confusing properties
5. key Ideas:
	* How do we identify things we talk about in the network
	* Whay do we need to identify(not so important now.. beasue we identify lots and lots of thing)
	* part of naming

## Problem and Terminology
1. Problem:
	1. Association between objects and names is too tight.
	2. Too few well defined concepts at hand.
2. The concept of OS: Binding
	* A name is what you want
		* why use name?
		* to identify things
		* want different name for different
		* easy to remember
		* things can change(same name can map to the current place)
	* An address is where it is
	* An route is a way to get there
	* Binding: mapping name to an address
	* context(by Neuman,1989): background about a binding

## Listing types of destination and binding:
1. 4 kinds of destinantion:
	* Service
	* Node
	* Network Attachment
	* Path(Route)
2. 2 obeservations: form and binding
3. Form Part:
	* More than 1 form of name for single type of objects
	* "name" broadly encompasses both forms(binary-digital and character)
	* not to associate some form with particular types of objects
4. Binding Part:
	* 3 kind of bindings:
		* Service to Node
		* Node to Attachment Point
		* Attachment Point to Route
	* Why binding?
		* need to resolve a name to an address
			* acctully, a string can both be a name or an address based on different context
		* for secure, give trust to go to the right place
		* Preserving identity when moving
	* Seend a data to a service need to discover 3 binding:
		* **Service name resolution**: a node run the service
		* **Node Name Location:** a attachment point that node is connected
		* **Route Service**: a path to the attachment point
5. The final binding choice may recorded outside the binding services within the network
6. What exactly one record means and which part of record can be changed

## Real-World Examples:
1. Ethernet: node and attachment point have the same name
	* pro:
		* node can physically move without any change in network record
		* one level of binding talbes is omitted
		* In two Ethernet, one node can present the same attachment point name
	* con:
		* one node have 2 attachment points and need to direct a message to one rather than the other
		* harder to accomplish
2. ARPANET: confusing about the name of node with name of service(due to mnemonics)
3. 3 conceptually distinct binding services may not be mechanically distinct:
	* name service: input service name and return list of network attachment points(Combind binding 1st and 2nd)

## Correspondence with Name, Address and Routes
1. Any of the four kinds of object may have a name
2. The address of an obejct is a name of the object it is bound to
3. A route is a more sophisticated concept include a path to the attachment point and some identificaiton of which activity within that node


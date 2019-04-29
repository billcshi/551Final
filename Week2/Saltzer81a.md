# Note For Saltzer81a

## Overview
1. End-to-End Argument:
	* A function can only be solve with the knowledge and help of the high-level(application level).
2. Proof that the low-level redundant for system crash recovery is little value.
	* particularly if the lower layers have a cost to do
	* if the app doesn't need it, it still need to pay for the cost
3. key Ideas:
	* where to put the function?
	* don't duplicate function at multiple level if it must be done at the end.
	* Why? the app is the only one that can guarantee it.
	* However putting it at lower level may improve the performance

## Example:Careful File Transfer
1. Defind the problem and transfer process(In Section 2.1)
2. 5 possible fault during the transfer process:
	1. hardware faults in the disk storage system
	2. mistake in copying and buffering
	3. transient error in processor or memory
	4. fault in communication system(Network)
	5. host crash during the process
3. Non end-to-end solution: Duplicate copies, unconomicial
4. End-to-End check and retry:
	* reduce the total time of retry(compare with non end-to-end solution)
	* The checksum is kind of must-do section due to the fault 1,2,3,5, even the network is fully garuantee
5. The low-level reliability is still improtant
	* the fault rate of a file transfer will exponentially increase with the file length increase
	* low-level duplication may hurt performance:
		* this check has a cost
	* it may help performance:
		* reduce the number of retransmissions?
6. But the low-level does not need to provide "perfect" reliability
7. In real-world problem:(Guildline)
	* Reliability measure is an engineering trade-off
	* The end-to-end check is not a need according to the reliable of network
	* **Better performance enhancement can be achieved at the high level**
	* **Every application need to pay for low-level function in a system, even it does not necessary need it**, so do some thing in low-level may not be cost efficiently.
	* Improvement of checksum is periodically compute and report the error earlier, however it seem to be a case to case function.
8. Defining the "End"(for WhatsApp)
	* left sending phone vs. device received vs. message read

## Other example
1. Delivery Guarantees
	* Why RFNM in ARPANET is not good?
	* End-To-End acknowledge is a need in some spectial case
2. Secure Transmission of Data(Encryption)
	* Why not in low-level?
		* securely manage the key
		* data is vulnerable between the interface of network and the application
		* authenticity still be checked by application
	* Why in Application level? More safety.
	* Lower-level encrption: Work as a firewall and can use a common key
3. Duplicate Message Suppression:
	* Why?
		* some duplicate message is originated by application itself rather than the network fault, so it can only recognize by application
		* not need to do again in lower-level
4. FIFO Delivery(Ordered Delivery):
	* Only independent mechanish at High-level can deal with some FIFO request
5. Transaction Management(SWALLOW SYSTEM, a remote data storage system):
	* not need to suppress duplicate message
	* not need to provide delivery acknowledgment, significant reduce the host and network load 
6. WhaysApps:
	* dilivery confirmation:
		* multiple confirmations
		* when should you get the notificaiton?
		* when the message is read is more end-to-end
7. Online Shopping:
	* confirmation the order went through
	* payment accepted
	* shipped,arrived?

## Conclusion and history
1. How to use end-to-end method is based on the need of application
2. End-to-End arguments is really similar to :
	* RISC
	* kernelization OS
	* Occam's razor
3. End-to-End argument may be part of the layered system
4. Why End-to-End Now?
	* smarter end devices enable end-to-end processing

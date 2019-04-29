# Note For Jacobson12a

## NABC Overview

* Need: Building a network layer based on Name Content to replace IP layer.
  * Why IP layer is not a good choice nowadays?
    * In 1960s, the problem of networking aimed to solve was resource(limited memory and disk) sharing. So the communication is conversation between exactly two machines.
    * Nowadays, people value the Internet with **what content** it contains.
    * **Named data** is a better abstraction than **named host**.
  * Important value for named data traffic:
    * Availability: Fast and reliable content access.
    * Security: Trust in content.
    * Location-dependency: Mapping content to host locations.
  * The success of IP layer:
    * The simplicity of its network layer
    * The weak demands it makes on layer 2: Stateless. unreliable, unordered, best-effort delivery.
* Approach: CCN network architecture
  * Two CCN packet types:
    * *Interest*
    * *Data*
  * CCN Naming
    * hierarchical set of labels
    * contents can be opaque to the network
    * but have meaning to the users and applications
    * like URLs, the start with a domain name to get uniqueness
  * Step of CCN conversation:
    * Consumer **broadcasting** its **Interest** over all available interface.
    * Any node hearing the **Interest** and having data that satisfies it can respond with a **Data** packet.
      * Satisfy means: the *ContentName* in the Interest packet is a **prefix** of the *ContentName* in Data packet.
      * So CCN can use any fast lookup method that IP router used.
    * Here is how CCN Node **respond to Interest**:
      * A packet arrive on a face(interface)
      * a longest-match look-up is done on its name
      * an action is performed based on the result of that look-up
  * The structure of CCN Node:
    * FIB(Forwarding Information Based): Toward Interest packets toward potential source of matching Data.
    * ContentStore: Data cache
    * PIT: Tracks Interests forwarded upstream toward content sources. If two upstream links share the same Interest packet, it will add to a same entry in PIT.
  * CCN **look-up process for Interest packet**:
    * Here is how CCN to avoid looping.
    * Check ContentStore, and reply if the Data is already in the memory.
    * Check PIT to avoid looping.
    * Check FIB to forward the packet.
    * Drop the left packet.
  * CCN **look-up process for Data packet**:
    * Check the PIT and forward the packet according to PIT.
    * If the PIT entry is timeout or non-found, drop the packet.
  * How to deal with trustiness(Key) in CCN security: Using public and private key to deal with provenance.
  * Property of CCN:
    * **One Interest retrieves at most one Data packet**. So the Interest serve the role of window advertisements in TCP and balance the flow.
    * CCN is linked locally, only between two nodes **in one hop**. The flow balance is maintained at each hop.
    * CCN can **take full advantage of multiple interface** without loop.
    * CCN using public key to ensure the integrity and provenance(the source of information) of the packet.
    * CCN can easily deal with DDOS since the multiple same Interest will response at the same time.
* Benefit: CCN has better performance than TCP/IP layer:
  * For point-to-point traffic:
    * The TCP can reach its throughput asymptote(Maximum throughput) faster than CCN
    * CCN can reach its maximum throughput
    * But CCN have a larger header overhead than TCP, so the total goodput is less than TCP
  * For **Content distribution** traffic:
    * Content distribution traffic is the majority Internet traffic type nowadays. Since the large content providers(likes Netflix and Google) domain the Internet traffic.
    * Since the CCN shared and cached the name data in its buffer, with the number of sinks increases, CCN performance stays constant.
    * TCP need to transmit as many times as the sinks, so the completion time increases linearly.
    * CCN is much better than TCP in content distribution traffic if **many users share the data**.
  * Voice over CCN and the fault tolerant of strategy layer
    * CCN can probe(by broadcasting) the network and find the best face to transit data.
    * Even manually disconnecting the cables, CCN will choose other link to transit data automatically.
    * Even with manually disconnection, a few packets were delayed but none were lost.
  * CCN with normal router:
    * **Any routing scheme that works well with IP should also work well with CCN.** They used the same longest match protocol and the binary hierarchical name(address) system.
    * CCN can be **incrementally deployed**  to replace IP.
    * CCN can be **layered over anything** including IP itself.
* Challenge: Since CCN need to deal with heavy header overhead, the delay will be higher(need to deal with signature) and the goodput is relatively bad.
  * The other thing is CCN need to broadcast its ***Interest*** packet which is somehow inefficiency.



## Other Information

1. Relation with *Intanagonwiwat00a*(Week 7):
   * These two paper both talk about data driven network.
   * Both paper used caching to deal with multiple sinks request the same data.
   * The CCN is stateless and linkless, but the sensor network(in Intanagonwiwat00a) using reinforcement to build a link.
   * CCN have router(CCN Node) but sensor network doesn't.
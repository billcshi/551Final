# Note for Schlinker17a

## NABC Overview

* Need: A traffic engine to **split** the **massive** egress(output) volume of **a specific prefix** from **giant content provider** (likes, Facebook and Google) to the edge network through **multiple peering routers** (multiple links) is needed.
  * The Basic Idea of Edge Fabric(The CDN traffic engine):
    * According to basic BGP protocol, egress router in content provider will choose one of the best(by shortest AS path or high local_pref, etc.)  peering router to output all the traffic for a single prefix.
    * However, since the traffic is very large, using a single peering router may cause significantly congestion
    * So, we need a traffic engine to split the traffic to several peering routers
  * What is PoPs(Points of Presence)?
    * A PoP has Peering Routers(PR), Aggregation Switches(ASW), and edge servers.
    * Why PoP? The cache content to server users directly and PoP split TCP from users to data centers.
    * A **global load balancing system** to assign users to PoPs via DNS.
    * PoPs connections to other AS via **transit provider** and **peers providers**. In some cases, a single **PoP have several different paths to access a specific prefix**.
    * **Locality**: In this paper, the author configure the network to **egress** a flow **only at** the PoP that **the flow enter at**.
    * **The Locality reduce the backbone network utilization, simplifies routing decisions and improves system stability.**
  * Why general **BGP and ECMP is bad**?
    * ECMP can deal with some of the traffic split situation but not in this case:
      * Each peering router may have **different capacity** and **different congestion states**.
    * BGP is not capacity-aware, but peering capacity is limited. 
      * Short-term spikes in demand or reductions in capacity cause volatility in demand and available capacity.
      * PoPs serve nearby users(according to global load balancing system), they share a daily pattern for traffic.
      * **ECMP** **cannot handle the routers with different capacity**.
      * According to the measurement, some router is significantly overloaded.(10% of routers experience a period in which BGP policy would lead to assign twice as much traffic as the interface's capacity.)
    * BGP decisions can hurt performance:
      * BGP protocol always make decision according to some stationary data(AS path length) .
      * However some dynamic data(likes congestion level) will affect the performance and make the second choice of BGP is much better then the first choice in some measurement(likes latency).
* Approach: Edge Fabric(CDN traffic Engine)
  * The Goal of the design:
    * Overcome BGP's limitation and use its interdomain connectivity to improve performance.
    * Decision must be aware of capacity, utilization and demand.
      * Watch load over different links and to avoid any link being congested
    * Decision should also consider performance information and changes.
  * Algorithm Structure:
    * Figure out all possible paths
    * Allocate to best links(greedy) that are not over capacity
    * For performance-aware, for each prefix, EF compare latency and goodput of the 2nd and 3rd choice path with the current paths. If the 2nd choice is better, it redirect the traffic to the 2nd choice.
    * Continue until all traffic goes somewhere
  * Operation on a per-PoP basis. The egress PoP is the same as the ingress PoP.
    * Edge-based routing:
      * Using host-based routing will consume a large amount of traffic in backbone network.
      * The balancing across multiple PoPs is done by global load balance system.
      * To achieve some good property of host-based routing:
        * Host can add a bit in packet header, and engine only work with the marked packet. So the host can decide which packet need to be controlled.
      * Engine only overrides a small portions of traffic.
  * Centralize control with SDN:
    * Stateless SDN control:
      * Using *stateful* control need a hard process to recover from updating and restart.
  * Balancing Routers according to their performance and capacity:
    * Why not using ECMP? Different routers have different capacities.
    * Why not using WCMP? Not every router enable WCMP and WCMP cannot deal with a subset of routes.
  * Using BGP for routing and control:
    * The Engine only overwrite the entry of BGP with different *local_pref* value to control which router to be chosen,
    * The router will deal with packet with BGP protocol.
  * Step of the Engine:
    * Capturing Network State: Get both routing information(capacity and utilization) and traffic information(current traffic rate, interface information and interface utilization).
    * Generating Overrides: Produce decisions according to the traffic detail and the network state.
    * Enacting Allocator Overrides: Output the decision to the BGP tables.
    * Monitoring
  * The paper also presents a method to **passive monitor** the network performance state.
    * How to place specific traffic to an alternate path by changing the IP header's DSCP field.
    * How to measure performance of alternate path.
    * How to use the measurement data to improve the Engine.
    * **The cool thing is Facebook already sending data to every phone**, so they don't need to probe the network by active measurement. They can use operational data to monitor the traffic.
* Benefit: Edge Fabric Engine provide a better performance than the BGP and the former stateful engine.
  * Decreasing the peck utilization for peer router.
  * Decreasing the performance digression in congestion hour.
* Challenge: The Engine is hard to deal with the improvement of capacity in peering router.



## Note during course

1. Problem and Solution:
   * Capacity: balance amount of traffic want to send vs. link bitrate
   * Performance: optimize latency
   * What BGP is missing:
     * need to consider multiple paths
     * doesn't consider load
   * Running frequently: every 30 s
   * Finding all paths:
     * gather all BGP control packet from each peer for their algorithm
     * they simulate BGP to find all possible routes
2. 


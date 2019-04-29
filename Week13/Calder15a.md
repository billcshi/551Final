# Note For Calder15a

## NABC Overview

* Need: Microsoft want to measure the performance of its global, latency-sensitive, **Anycast CDN**.
  * Different between **Anycast CDN** and **DNS-based CDN**:
    * DNS-based CDN:
      * CDN makes a performance-based decision about what IP address to return based on which LDNS(local DNS resolver, **the Recursive Resolver**) forwarded the request.
      * DNS redirection allows relatively **precise** control to redirect clients on **small timescales** by using **small DNS cache TTL value**.
      * Making decisions at the **granularity of LDNS**.
        * It is a big issue for DNS-based CDN, since some public DNS resolver(likes Google DNS) servers users all over the world.
        * A proposed solution to this issue is **the EDNS client-subnet-prefix standard(ECS)**.
        * **ECS** requires **investment in infrastructure and operations for DNS provider.**
    * Anycast CDN:
      * **Multiple Server Nodes** in **different locations** provide **the same IP address** to DNS.
      * Based on **BGP protocol**, the network will choose the shortest BGP route to a nearby Server Nodes.
      * **Each client redirection is handled independently** by BGP router.
      * Still have **some challenge**:
        * Anycast is unaware of network performance.
        * Anycast is unaware of server load.
        * Anycast routing changes can **cause ongoing TCP to terminate** and need to restart.
          * Since CDN is usually used in Web application, the TCP flow is always short.
  * This paper try to answer two questions in Anycast CDN measurement:
    * How effective of Anycast CDN?
    * How does Anycast performance compare against DNS-based CDN?
* Approach: This paper used **passive and active measurements** to answer these two problems.
  * Passive Measurement: Using server log about client IP address, location and what front-end was used.
  * Active Measurement: Using JavaScript beacon to compare Anycast choice and other 3 choices(nearest nodes and 2 random nodes).
    * For each client, choice 10 nearest front-end nodes as candidates.
      * Proof by dataset in Section 3.3, **after choosing 5th nearest nodes the improvement to latency is almost zero**.
    * Choice 4 front-end from 10 candidates for measurement:
      * The Anycast choice
      * The nearest choice
      * Random 2 nodes from the other 9 candidates
    * Aggregate measurement by /24 prefix to provide a robust measurement result.
* Benefit: The Anycast CDN performance is optimal in most(80%) case. However, in 20% cases, Anycast CDN provide the suboptimal choice.
  * Same example of poor Anycast routes:
    * Since Anycast based on BGP protocol, there is no way to communicate this internal topology information in a BGP announcement.
    * Interdomain routing policies of ISPs select remote peering points.
  * In 87% of cases the Anycast still choose a **very near front-end**.
  * The Anycast **doesn't** performance **consistently** poor.
  * The Front-end has affinity in Anycast CDN network.
* Challenge:Using Anycast CDN cannot provide better performance than the DNS-based CDN with ECS.
  * The author proposed a possible solution which hybrid approach that combines Anycast with DNS-based redirection.
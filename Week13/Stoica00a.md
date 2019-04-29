# Note For Stoica00a

## NABC Overview

* Need: An efficiency algorithm is needed to locate the servers nodes by hash keys in peer-to-peer systems.
  * Peer-to-peer systems and distributed systems:
    * Map a key(a 160 bits number) to its "value" at some location
      * a key could be a hash of something with meaning
    * Without centralized control
    * Without hierarchical network structure
    * The software running at each node is equivalent in functionality
    * *Nodes can come and go at any time*
  * Compare with other key-nodes system:
    * DNS: Provides a host name to IP address. Chord can also do this approach.
    * Freenet and Freenet likes system: Have a degree of anonymity, but prevents it from guaranteeing retrieval of existing documents or from providing low bounds on retrieval cost.
    * Can system: Using a d-dimensional Cartesian coordinate space. The lookup cost is O(d`N^1/d). The Chord used d=logN, which can be prove to be faster.
      * Also Can used a hierarchical structure but Chord used a flat structure.
* Approach:Chord Lookup System
  * The problem of Chord need to solve:
    * Load Balance: Spreading keys evenly over the nodes.
    * Decentralization: Each nodes are same
    * Scalability: Since the number of nodes can be very large(Thousands of Nodes). The lookup speed of Chord grows as the log of the number of nodes.
    * Availability:  Need to support add new nodes and nodes failure.
    * Flexible Naming: No constraints on the structure of the keys it looks up.
  * Deal with **Naming**:
    * Using **SHA-1 to hash** the value-identifiers and node-identifiers to a m-bits binary value
    * It can deal with all kinds of value structures and nodes
  * Deal with **Load Balance**:
    * Using **consistent hashing** to deal with load balance.
    * In Section 4.2 describe a method to implement consistent hashing in distributed system
    * The most important part is:
      * **Maintaining** a **successor(k)** function to find the **successor node** for key **k**
      * Also maintaining a **predecessor(k)** to find the previous node on the identifier circle
      * (in a practical system: a couple of backup successors in case your successor leaves)
  * Deal with **Scalability**:
    * Using a **finger table** to preform a **shortcut** of the identifier circle.
      * Finger table structure the identifier circle like a jump table(or binary search)
    * The time complexity of finding a key in a N node system using finger table is O(LogN)
    * The pseudocode and proof of the algorithm is listed in Section 4.3.
  * Deal with **Availability**:
    * The pseudocode of Joins listed in Section 4.4
      * The message send to other node is O(Log^2 N). These messages are used to maintain the finger table.
      * Initializing the fingers table of the new node is also O(Log^2 N) (in a naively approach) and O(Log N) (by a practical optimization)
    * The pseudocode of Stabilization listed in Section 5.1
      * Stabilization is used to **maintain the successor(k)** function.
      * Stabilization take times to converge to the true function
      * It can be proofed that if successor(k) function provides the true nodes, the finger table can converge to the true table.
  * Basic **Join** Algorithm:
    * Pick an id: by hash of a random number(IP address, etc.)
    * Insert yourself into ring:
      * search for your own id
      * find your successor
      * tell that successor you are the new predecessor
      * find your predecessor
      * take over any keys you are responsible for
      * Done!
    * Build the finger table
    * but still need to Update everyone's finger table(that should point to you)
      * stabilization runs periodically at every single node and checks all of its finger entries
      * and all finger table are **soft state**: we check that they are correct every time we use them and fix if they are not
* Benefit: The Chord algorithm is measured by simulator and experimental testbed.
  * In Simulator:
    * Consistent hash and finger table provides **O(Log N) complexity for Lookup algorithm**.
    * Using virtual nodes to improve the resource balancing in network.
    * The **average path length of looking-up is O(1/2 Log N)**
    * The failure rate during stabilization process is relatively small.
  * In Experimental Testbed:
    * The lookup time in real-world situation is also very small.
    * The author **run several software in a single node to test how well the implementation scales.** 
* Challenge: Chord cannot deal with partitioned rings. Also the number of background messages sends in the process is relatively large. (Need Log^2 N messages for maintaining.)
  * Since Chord is a peer-to-peer system, some nodes may be a bad actors, but Chord ignores this.
  * For performance model, each node is equally far away but the latency between nodes can vary a lot in real-world application.
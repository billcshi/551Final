# Note For Greenberg09a

## NABC Overview

* Need: A Data Center network structure is needed:
  * Basic Needed: The **Agility**.
    * Agility promises improved risk management and cost saving.
    * Data center operator can meet the fluctuating demands of individual services from a large shared server pool, result in high server utilization and lower cost.
  * Problem in nowadays data center network:
    * Existing architectures(tree-like hierarchy architecture) don't provide enough capacity between the servers they interconnect.
      * The path to highest level is significantly over-subscription. 
    * The network does little to prevent a traffic flood in one service from affecting the other services around it.
      * To overcome the reliability issue, the basic resilience model is 1:1, so the maximum utilization is 50%.
    * The routing design in conventional network achieves scales by **assigning servers topologically significant IP addresses** and **dividing servers among VLANs**.
  * Key Idea of VL2:
    * Give each service the illusion that all the service assigned to it are connected by a single non-interfering Ethernet switch(*Virtual Layer 2*) and maintain this illusion even as the size of each service varies.  **Just like using a (virtual) LAN.**
  * 3 Objective for these structure:
    * Uniform high capacity: traffic should be limited only by the available capacity on the network-interface cards of the input and output server.
      * Uniform means all server-to-all server
    * Performance isolation: Traffic of one service should not be affected by the traffic of any other service. (one datacenter tenant's traffic should not affect others)
    * Layer-2 semantics: Agility
  * Some other limitation:
    * make no changes to the hardware of the switches or servers
    * legacy applications work unmodified.
    * No new switch software or APIs are needed.
* Approach: The author first analyze the traffic patterns in a production data center. Then the author design, build and deploy the VL2 and validate VL2. After that he apply VLB(randomize) to split the flow-level traffic. Finally he measure the performance.
  * Datacenter Network structure:
    * top-of-rack switches: in a rack
    * aggregation switches: between racks
    * access routers: out of the datacenter
      * with proxies or some kind of address mapping
    * access control mechanisms and firewall: isolation of tenants from each other
  * The data pattern in data center is **highly divergent**(even over 50 representative traffic matrices only loosely cover the actual traffic matrices.) and they change **rapidly and unpredictably**.
    * The hierarchical topology is unreliable.
  * Design principle for **VL2**:
    * Randomizing to Cope with Volatility: Using VLB and ECMP to do random traffic spreading across multiple intermediate nodes.
      * VLB distributes traffic across a set of intermediate nodes.
      * ECMP distributes across equal-cost paths.
    * Building on proven networking technology: Using IP and TCP to deal with some cases.
    * Separating names from location: To cope with agility.
      * VL2 separates **server name**, **application address**(AA) and **location address**(LA)
      * Each AA is associated with an LA .
      * The VL2 **directory system** stores the mapping of AA to LA.
      * The traffic between server is using AA address and the switches translate the AA to LA in underlying level.
      * The directory system control the translate process.
    * Embracing End System: The component of VL2 can be replacement by ad-hoc technique.
    * **Scale-out Topologies**: Using simple devices to build a broad network.
      * **Building a Clos network.**
* Benefit: The paper evaluate VL2 using a prototype and found it match the design principle.
  * High Capacity and good fairness.
  * Provides VLB Fairness: The traffic is split almost equally across the links and intermediate switches.
  * Isolation: Not hot spots exists in the network(due to randomization)
  * Convergence after link failures.
* Challenge: The overhead of VL2 is still large.
# Note for Govindan16a

## NABC Overview

* Need: Maintaining the highest levels of availability for large content provider(likes Google) is challenging in face of scale, network evolution and complexity.
  * The challenge in maintaining high-availability rate for Google:
    * Scale and heterogeneity: 
      * Google's network spans the entire global and the network have very different components.
    * Velocity of evolution: 
      * According to its **Continuously Upgrade the Network** principle, Google's network upgrade continuously and incrementally. So the network need to keep in high-availability with upgrade the network daily.
    * Device Management complexity: 
      * The network control plane is also evolving all the time.
    * Constraints Imposed by Tight Availability Targets:
      * A long upgrade process introduces a substantial windows of vulnerability to concurrent failures.
  * The network structure of Google:
    * Which have been discussed in detail in class papers:
      * [Jain13a] (Week11) about B4 network architecture
      * [Bosshart14a] (Week10) about P4 SDN control
      * [Dean13a] about Google's tail-tolerant flamework and [Dean04a] about Map/Reduce (Week 9)
      * [Cardwell17a] about BBR implementation of TCP protocol.
    * Three components of Google's Network:
      * A set of campuses where each campus hosts a number of clusters
        * Consist of logically centralized control plane
          * Fabric Controller
          * Routing Agent
          * OpenFlow Controller
      * B2, WAN between user interface and clusters
        * Control just like normal ISP
      * B4, WAN for carrying traffic among clusters.
        * Within a B4BR, control just like cluster
        * Across B4BR, control plane consist 4 components:
          * B4 Gateway
          * Topology Modeler
          * TE Server
          * Bandwidth Enforcer
    * Traffic control by priority:
      * User-facing services have higher priority than internal customers services
      * The priority of traffic also influences the mechanism to deal with failure.
* Approach: This paper discuss the mechanism to local the failure and find the root causes of these failure. Then it sperate different failure's root causes in to several categories by the plane it belongs to. After that the author proposed some principles for high-availability network designing.
  * **Baseline** Availability Mechanisms
    * Clusters and WAN topologies are carefully capacity planned to accommodate projected demand.
    * Every logically centralized control plane component is **replicated**.
    * **Offline approval process** by which services register for specific traffic priorities.
    * Several management plane processes designed to minimize the risk of failures.
  * **Post-mortem reports**
    * Record almost every thing about the failure event
    * Analyze carefully about the event log and documented in a post-mortem
  * In Section 5, author provide statistic analyze of failure events and category of the root causes.
    * By network and plane type, Structural element, Impact and Duration
  * In Section 6, author analyze different events and the root causes.
  * Design principles for high-availability:
    * **Defense in depth**:
      * *Contain failure radium*:
        * logically partition the topology
        * assign each partition to a distinct set of control plane elements
      * *Develop fallback strategies*:
        * If the high-level mechanism failure, fall back to the low-level mechanism
        * For example if the SDN fail, go back to normal switches
    * **Maintain Consistency** within and across Plane
      * *Update network elements consistently*:
        * Control plane and data plane synchronization by hardening the control plane stack.
      * *Continuously monitor network operational invariants*:
      * *Require Both the Positive and Negative*:
        * Draining a large number of devices requires specifying:
          * all of the devices to be drained
          * **an explicit, separate list of devices to be left undrained**
      * *Management Homogeneity with System Heterogeneity*:
        * A unified and homogeneous network management architecture to deal with 3 different structure.
    * **Fail Open**:
      * Preserve as much of the data plane as possible when the control plane fails.
      * *Preserve the data plane*:
        * When control plane fails, it doesn't delete the data plane state.
      * *Verify large control plane update*:
    * **An Ounce of Prevention**:
      * *Assess Risk Continuously*:
        * Not only at the beginning, but also during the process.
      * *Canary*:
        * Run at a few machine firstly.
    * **Recover Fast**
    * **Continuously Upgrade the Network**
      * Continuously and incrementally upgrade is better than a single "big band" upgrade.
* Benefit:  These principles and mechanism help Google to maintain its network server in high-availability level.
  * The design goal of Google's high-availability design:
    * No more than a few minutes downtime per month.
* Challenge: Since Google's network server is extremely large to maintain, even these principles work for some cases, there still in some cases the server will go down and impact the quality of service.
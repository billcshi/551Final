# Note For Dean13a

## NABC Overview

* Need: Large-scale Web services used thousands of different machines. Due to the hardware and software differentiate between these machine, the variability of latencies across these machines exist. The slowest part of machines will significantly decrease the performance of the whole system. A latency tail-tolerant flamework is needed to deal with this situation.
  * The reason of variability:
    * Shared resources: the same application different request might contend for resources
    * Daemons
    * Global resource sharing
    * Maintenance activities: Garbage-collection process can cause periodic spikes in latency
    * Queueing: the request queue length is variety in different machines
    * Power limits: CPU cool down
    * Energy management: Due to Power-saving protocol, some component will turn off when idle and need time to restart.
  * The variability affect the performance:
    * A basic assumption is the latency in different machine is uncorrelated.
    * The latency across the whole system is heavy-tail.
    * The worst part of the system is magnified at the service level. 
* Approach: This paper analyze the cause of heavy tail in latency and give a solution for each cause in Short-Term(need immediate response) and Long-Term cases.
  * For queueing: short queuing in high-level and low-level
    * Keep low-level queues short so higher-level policies take effect more quickly.
    * **Hedged Request**: Copy a request to another machine after a 95th percentile time of waiting, and if a machine return the answer, stop other machines.
      * We assume that **if a task take more than 95th percentile time**, it should be something wrong in the first machine, so we send the same request to another machine.
    * **Tied Requests**(For short-term): Assign a request to 2 machine, and if one started(after queuing) cancel the other one. So in general every request uses a shorter queue between 2 machine.
  * Break long-running requests into a sequence of smaller requests which help the high priority request can execute faster.
  * **Micro-partition**: Split a task to more partitions than there are machines in the service.
    * The overhead is not very large.
    * We can assign the partitions according to the variety performance of machine.
    * Failure-recovery speed is also improved. 
    * **Selective replication:** making additional copies of popular and important documents in multiple micro-partition
  * Latency-induced **probation**: The master need to send probe to check if a machine is working in good latency. And stop to send task if the machine states is wrong. And send shadow probe to restart the machine states.
  * **Good Enough** Answer
  * **Canary request:** For untested code, the master send to 1 or 2 machine at first, and if it run correctly, it send the rest of request to the machines.
* Benefit: These approach decrease the latency of whole system.
* Challenge: The overhead of these approach is relatively high. A key idea of these approaches is using replication of the request across different machines to decrease the varieties of latency in single machine.



## Note on Course

1. Key Ideas
   1. reduce latency
   2. specifically the variability in latency
      * they want consistent performance
      * because extremes are very noticeable
      * **tail latency:** the tail of the distribution
   3. causes of variability
   4. application
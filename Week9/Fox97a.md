# Note For Fox97a

## NABC Overview

* Need: An incremental scalability, High Availability(High fault tolerance), Commodity Building Blocks Network Services architecture is needed for web-based application development.
* Approach: A Cluster-Based Network Services \& BASE Semantics:
  * What is Cluster-Based: Compare with SMP(Using a single large machine), Cluster-Based system used relatively small machine to cooperate.
    * Pro: Easy to increase scalability, High tolerance and every single machine is commodity building blocks(easy to buy)
    * Con:Administration, Component-level replication(Each component need to have well-circumscribed responsibilities), Need to handle partial failure in the whole system
  * What is BASE Semantics: Compare with ACID(Promised Database) Semantics(Which need every process share the same state), BASE semantics data tolerate different process have slightly different in share state.
    * In distributed system(Cluster Based system), if every data need to be ACID Semantics, the overhead in synchronous data is unaffordable.
    * In this paper's approach: It uses Stale data(old data until entry timeout), soft state and approximate answers to handle the BASE semantic data.
  * Service Architecture: 
    * Basic Idea: Decomposition the application function to different components**(Front End, Worker Pool, Customization Databased, Manager, Graphical Monitor, System-Area Network)**. Each component is confined to one node.
    * One cool thing is: The system balances the load using a center manager.
    * Another thing is: For fault tolerant, the architecture used *process peer fault tolerance*, another node to monitor the node and restarts it when fail. 
    * TACC: A Programming Model(Library of standard tools) for Web APP. **Allow the same workers(Node) to be reused for different services.**
  * For Fault Tolerance:
    * process peer fault tolerance
      * one process watches the other and takes over after failure
    * monitor current load
  * Soft State:
    * just keep state in the memory(rather than disk, Hard state)
    * recovery after failure and regular operation is the same code: Just wait for them to broadcast the state
* Benefit: The Cluster-Based service is a good architecture to deal with massive web-application and the author did implement and validate several example in his paper.
* Challenge: Since the web-application is much more larger than the application in 1997. A single machine(PC) to handle the manager in the system seems to be impossible, how to deal with the distributed manager still a big problem.
  * Why centralized monitor in the paper?
    * easy to start a new one with software
    * centralized monitor is enough and we know how to recover
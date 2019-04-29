# Note For Dean04a

## NABC Overview

* Need: A programming model to simplify coding on large scale clusters-based distributed systems is needed.
  * A cool thing is: Even the parallel programming is hard for some tasks, for most basic tasks you can use really simple **restricting model** to express.(Only 2 method **Map and Reduce** is used in this paper to represent variety of tasks.)
* Approach:  A programming model based on 2 functions: **Map and Reduce** 
  * Implementation of Excution:
    1. splits the input to 16MB~64MB files
    2. One of the machine become **Master(Single Master for a task, it need to record stage of all map and reduce tasks and the state of each worker),** Master picks **idle workers** to map task or reduce task. 
    3. *Map task worker* read the **corresponding input split file** and calculate the **map function**, write to local buffer.
    4. Periodically, **map task worker** write the buffer to local disk. The locations of the local disk **passed back to master.** Master sent these data **to reduce workers**.
    5. **Reduce workers** read(*remote read*) all intermediate date from map task workers' local disk. The Reduce workers also **sort** the data **by intermediate key**.
    6. Reduce workers use the sorted data with reduce function.
    7. After all map tasks and reduce tasks have been completed, the master **wakes up the user program**.
  * Fault Tolerance: More than a thousand of machines used in these system, so it need to deal with partial fault
    1. Worker Fault: The master will re-execute the task assign to a different machine.
    2. Master Fault: Since the only master in the system, it is very rare to fault, so by default the implementation aborts the master fault. 
    3. System Lock: By atomic rename function on file system.
  * Other Refine Function:
    * Locality: Network bandwidth is relatively scarce resource, so each machine used input file according to location information.
    * Task Granularity: The master must make O(M+R) scheduling decisions and keeps O(M*R) state in memory.
    * Backup Tasks: To deal with **"straggler"**, the master will schedules backup executions of the remaining in-progress tasks.
    * **Skipping Bad Records**
    * Counters Function
* Benefit: MapReduce is well expressive and very simple for variety of tasks. The performance of these implementation is very good.
* Challenge: MapReduce only have 2 function and some of the tasks cannot be represent by this programming model.



## Course Note

1. Key Ideas
   * (re)defined map/reduce(although the idea was in Lisp 40 years ago)
     * input key-value pairs, output other key-value paires
     * map: apply to input data
     * reduce: input is all values with the same key
   * easy-to-use programming
     * just write map and reduce functions
     * it does the parallelism
     * it applies to a lot of applications
   * parallel processing
     * split the input, run mappers in parallel and run reducers in parallel
     * fault tolerant
   * network of lots of different machines
   * target app is batch processing
2. How to do shuffle(all-to-all process)?
   1. hash into bucket, then sort each bucket
   2. merge all mapper output together
   3. every mapper has to coordinate with every other mapper and every reducer
   4. In practically: We split the mapper output in mapper code and we merge the input of reducer in reducer code
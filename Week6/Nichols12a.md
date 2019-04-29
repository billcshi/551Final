# Note For Nichols12a
## NABC Overview
* Need: Since the buffer of the router is relatively large, the increment of queue length significantly increases the delay of the network. A simple, robust queue algorithm that can manage buffer delay without a negative effect on utilization is needed.
	* Some design criterias of the algorithm:
		* Parameterless
		* Treating good and bad queues differently
		* Controling delay
		* Adapting to dynamically changing link rates with no negative impact on utilization(throughput)
		* Simple and Efficient(can implement in different kinds of backend)
* Approach： Design the CoDel algorithm and compare with RED and tail-drop algorithms in the simulation.
	* CoDel Algorithm's Detail:
		* Focus on local **minimum queue**
		* Using **packet-sojourn time** to measure the queue size
		* Keep a single variable of how long the minimum queue has been above or below the **target size**
		* When the **queue size** exceeded **target** for at least **interval**, CoDel start to drop packet until the **queue size less than target or the queue is less than 1 MTU**
		* **Target** and **Interval** are constant in algorithm
* Benefit: From simulation:
	* CoDel provide lower delay than tail-drop algorithm and almost keep the utilization almost 100%
    * Provide higher utilization than RED algorithm in all kind of network.s
    * In dynamic network, CoDel is much better than RED algorithm since it respond to changes quickly.
    * Packet drop is more fair than RED, according to Jain fairness index.
    * CoDel algorithm can also implement in Consumer Edge
    * CoDel fit different implementation TCP in source mechine
* Challenge: CoDel cannot work very well in Cable Modem and edge network

## Other Important Information
1. good queue and bad queue(from Page 4)
	* good queue: acting as shock absorbers to convert bursty arrivals into smooth, steady departures
	* bad queue: doing nothing(with throughput) but creating excess delay
2. Jain fairness index(from Page 6)
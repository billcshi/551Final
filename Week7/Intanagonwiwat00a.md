# Note For Intanagonwiwat00a

## NABC Overview
* Need: With the improvement of the sensor network, the need for an energy efficient, robust, scalable network protocol for these devices is emerging.
* Approach: This paper designs and simulates the Directed Diffusion protocol.
  * Naming Data: Task and reply are named by a list of attribute-value pair. The choice of the naming scheme can affect the expressivity of tasks.
  * For each sensor, it records an *entry list* for *interests*. 
  * In each entry, including these data:
  	* Cache of the former data
  	* The duration of the entry
  	* The timestamp of the entry
  	* The **gradient** of the entry
  * About the **gradient**: It records the data rate and controls the sensor to send or receive the data.
  * Sensor generate event samples **at the highest data rate**(according to gradient).
  * Sensor propagate the data according to the gradient.
  * Shortest Delay(Path) Finding by reinforcement and negative reinforcement in the network:
  	* All data collection section **started with low-data rate**. 
  	* After the first packet arrived, the sink(the human operator) increase the data rate(the gradient) to the nearest(first replier) sensor and decrease(or time out) the other sensor. 
  	* All propagate the reinforcement and negative reinforcement to its neighbor
  	* After the process, the shortest path has a high data rate and other path have a lower data rate.
  	* So data will send faster in the shortest path, and the other path will save energy due to IDLE.
  	* Due to other path still sending the data(in lower rate), if the shortest path is out, the network still working and the second shortest path will soon be reinforce to high rate.
* Benefit: The Directed Diffusion protocol is high energy efficient, lower delay and high robustness. 
* Challenge: These protocol is really based on the MAC layer protocol.



## Other Information

### Key Idea

1. diffusion can be more efficient than IP
   * less energy
   * all the nodes collaborate
2. In Internet we know the topology(with pre-compute routing table) and do routing
3. In diffusion: 
   * we don't know the topology, just know neighbors(**Localized interaction only**)
4. data-centric(**named data**)
  * naming the data with attributes rather than IP addresses in the Internet
5. uses reinforcement to discover "routes" for data
   * a user **floods** interests through the network to discover the sensors
6. How to deal with loop, duplicate path and flooding:
   * negative reinforcement and caches to suppress duplicate path
   * reinforcement and high-rate data avoids flooding every time
   * use location information: exposed to the network with attributes
7. Computation in the network(In-Net Processing):
   * can't do this in a general way in IP
   * some big companies put machines around the internet(Google) to do caching
   * In this paper: for duplicate suppression for energy saving

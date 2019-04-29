# Note For Katabi02a

## NABC Overview
* Need: This problem needs to be solved that with an increment of bandwidth and delay makes TCP protocol less efficient and unstable, no matter which queueing algorithm used. 
* Approach: XCP protocol
  * The Sender part:
    * Add a congestion control header to packet with current cwnd and rtt
  * The Receiver part:
    * Receive the packet and copy the congestion control header to  ACK packet 
  * The Router part:
    * Decouple fairness control and utilization control(EC)
      * maximize link utilization while minimizing **drop rate** and persistent queues
      * EC deals only with the aggregate behavior, it doesn’t care about which percents every user get
      * Basically uses Multiplicative-Increase Multiplicative-Decrease law
      * Unlike TCP increase slowly, XCP increase the aggregation throughput to the limited in one RTT
    * For fairness control(FC):
      * Apportion the feedback to individual packets to achieve fairness
      * Allocate increment for all flows the same amount(or based on their price)
      * Allocate decreasement for a flow proportional to its current throughput
      * The Additive-Increase Multiplicative-Decrease law convert the network to fairness(or priority based on their price)
    * Without Keeping per-flow state(Using the packet Header to store the value and control)
* Benefit: XCP outperforms TCP both in conventional and high bandwidth-delay environment
  * Compare XCP with TCP(with different queueing algorithms)
  	* The XCP always keep utility near optimal
  	* The XCP much less likely to drop any packet
  	* The XCP do well in a dynamic environment(convert to stable faster)
  	* (no per flow state)
  * XCP can deal with the ill-behavior source and deal with other normal TCP sources
* Challenge: Since XCP already change the header struct of the packet, this protocol needs to be implemented in all intermediate router. Otherwise the packet will not available in the network

# Some Other Idea

1. Challenge in 1990s:
  * signalling congestion was hard:
    * everyone has to help
    * backwards compatibility is hard
  * Why didn't they just enforce ECN?
2. The problem of TCP:
  * 3-way handshake and expensive connection setup(Especially for small packet)
  * retransmissions are hard
    * B(p) is a function of 1/RTT
    * high bandwidth-delay links => AIMD is strict and has big changes if you're going fast
3. Why AIMD need shuffle?
  * Even if full channel, still need to even out allocation
4. Why not XCP in 1990s?
  * router CPU were slow
5. Possitive feedback per flow and negative feedback per (send) rate:
  * additive increase per flow, multiplicative decrease per rate
  * large pkg => more thorghput => more feedback
  * short rtt => more packet => less feedback per packet

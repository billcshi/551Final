# Note For Bharghavan94a

## NABC Overview
* Need: Even though MACA resolve some of the plights in CSMA protocol, there are still some key problems to be solved in the media access protocol for wireless LAN's.
  * The contention is at the receiver, rather than the sender.
  * The congestion is location dependent.
  * The allocation of media access should be fairly.
  * The media access protocol should propagate synchronization information about contention periods.
* Approach: Analy the problem in MACA and modify MACA to MACAW.
  * Blackoff Algorithm: 
    * The basic idea of Blackoff: slow down to avoid collisions when you retry
    * The MACA algorithm makes the advantage link completely occupy the network, and the disadvantage link has extremely high Blackoff.
    * Firstly, MACAW let the sender include its Blackoff in RTS. Then receiver copy that number in CTS. Then every sender copy this number to its Blackoff counter. So every sender shares the same Blackoff number.
    * MACAW also modifies the rule for BO which multiplies by 1.5 when increase and minus by 1 when decrease.
    * To deal with no response machine in the network, the Blackoff number for different destination station in one sender should be separated and the same destination station should have the same Blackoff number in the different sender.
  * In multiple streams model, MACAW allocates the bandwidth equally to streams by using a random algorithm.
  * Link-Level ACK:
    * why? 
      * could depend on TCP to timeout and retry
      * can be faster: over shorter distance, know loss is due to corruption
  * In Massage sending:
    * Using ACK to replace CTS, when sending TCP and the ACK packet is lost
    * Using DS for a kind of *hidden terminal state*
    * Using RRTS for a kind of *exposed terminal state*
    * These three packet increase the throughput and fairness in the network, however, MACAW cannot solve all kinds of *hidden terminal state* and *exposed terminal state*(an example is listed in Figure 7)
* Benefit: The fairness of the algorithm is much better than MACA. Even the ACK, DS, RRTS packet use part of the network throughput; the throughput is still higher than MACA consider the congestion and the noise.
* Challenge: The MACAW cannot solve all kind of hidden terminal and exposed terminal problem.


## Other Informaiton
### Plight of CSMA(carrier sense) protocol
1. hidden terminal: collisions occur at the receiver
2. exposed terminal: unnecessary collision detection

### MACA protocol
1. The sender sends RTS packet to the receiver
2. If the receiver is IDLE, it replays CTS to the sender
3. After receiving the CTS packet, the sender starts to send DATA
4. If it has not received CTS packet, retransmissions occur after several slot times. The number is calculated by Backoff-counter(BO).
5. The BO doubles for each retransmission and decreases to 1 when success.

### Contention-based vs. token-based(scheduled) based

1. Contention-based
   1. each source fights for a chance to send
   2. could be simpler but easy to be unfair
2. Scheduled-based
   1. Everyone has a designated time to send
   2. easier to be fair
   3. not ideal if sources are moving, have to re-compute schedule
3. MACAW : contention-base, 802.11: both, 4G LTE: scheduled-based
4. Sometime change from contention-based to scheduled-based because it need to guarantee the receiver get the message

### Base-station vs. ad hoc

1. base-station
   1. device with (big) antenna, usually connect to wired networks
   2. hopefully base-station more reliable
   3. no coverage where it was not planned
2. ad hoc
   1. on-the-fly and peer-to-peer
   2. can work without pre-planning
   3. maybe don't work over long distance
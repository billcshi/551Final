## Note For Akhtar18a

## NABC Overview

* Need: An ABR algorithm to deal with video streaming in various network conditions.
  * ABR: adaptive bitrate algorithm
    * **Chop** a video into *chunks.*
    * **Choose** which bitrate level to fetch a chunk **based on conditions**.
  * QoE: User quality of experience
    * The factors that affect QoE: start up latency, average bitrate and rebuffering ratio
    * Goal for ABR algorithm: optimize QoE
      * Ensuring the bitrate seen by the user is as high as possible
      * Avoiding rebuffering events at the client.
    * Different between **QoS(from Shenker95a Paper) and QoE**:
      * QoS: **guarantees** about parameters like bitrate, loss, latency, jitter
      * QoE: user rating(happiness) of video
  * Former ABR algorithm:
    * MPC:
      * Based on network throughput prediction and buffer occupancy.
      * Using throughput to select bitrates to optimize a given QoE function.
    * BOLA:
      * Based on buffer occupancy.
      * Models bitrate selections as an optimization problem which it solves for a given value of the buffer.
    * HYB:
      * Based on throughput prediction without lookahead.
      * Selects bitrate using a simple heuristic.
    * Pensieve
      * Using deep reinforcement learning
    * Problem: Current ABR algorithm perform well on average, but some users can experience poor delivery performance. Because ABR algorithm have **limited dynamic range**.
  * **Main contribution** of this paper: **Improves the dynamic range** of ABR algorithm. And ensuring high QoE for **All Users**.
    * ABR algorithm have parameters(configurations).
    * Most current ABR algorithm used fixed parameters to deal with most cases but not every cases.
* Approach: This paper presents an OBOE algorithm.
  * OBOE have 2 stages:
    * **Offline stage**: Pre-computes the best configuration choice for each network state
    * **Online stage**: Detects changes in network state and applies the pre-computed best configuration(parameters) for the current state.
  * Why using **network state**?
    * The network throughput along a path **is not**  necessarily **a stationary process**.
    * TCP connection throughput **can** be modeled as a **piecewise stationary process.** 
    * **The network states is stationary** and often lasts for tens of minutes.
    * Using the mean and standard deviation to represent a network state.
  * Offline Stage:
    * This paper used 3 components to generate the best configuration for different network state
    * ConfigEvaluator:
      * Input a stationary throughput trace and ABR configuration.
      * Explore configuration space of an ABR algorithm on each network state.
      * Using **synthetic traces** rather than real traces.
    * VirtualPlayer:
      * Models the dynamics of an actual video player.
      * Input a throughput trace and output the QoE performance metrics.
    * ConfigSelector:
      * Maps a given network state to the best configuration.
  * Online Stage:
    * Using an **online change point detection algorithm** to dynamically change ABR configurations during a video playback.
    * The algorithm used is a Bayesian online probabilistic change-point detector.
      * For nonoverlapping state.
      * The algorithm is fast and requires no prior knowledge about the data stream.
    * Reconfiguring ABR algorithm when the network state change.
* Benefit: The author demonstrate three existing ABR algorithm with OBOE auto-tune parameters and compare with another ABR algorithm. The OBOE algorithm present a better performance. And also OBOE can improve different ABR algorithm.
  * Evaluation Metrics:
    * OBOE evaluation is to demonstrate the extent to which it can improve the underlying metrics that **an ABR algorithm is designed for**.
  * Noted that VirtualPlayer is also been verified during the evaluation.
  * The detailed of evaluation section is from Page 8 to Page 12.
  * A cool thing for OBOE is:
    * OBOE **can auto-tune different ABR algorithms** with different parameter meaning.
  * OBOE **Overhead**:
    * OBOE need to offline calculate the best parameters **for different network state**.
    * The offline process can be computer on cloud.
    * For the online part, the overhead of searching in the mapping table is relatively small.
    * The online searching process can also be moved to cloud.
* Challenge: Even the OBOE improve most sessions, some sessions' performance is decreasing, which is contrary to the design goal. Also how to transmit the ConfigMap(the table mapping the best parameters and the network states) is also a big problem.
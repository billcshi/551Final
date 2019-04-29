# Note for Leland94a

## NABC Overview
* Need：Several existed models about the Ethernet(Network) traffic are misleading the network simulation and examination. A more accurate model should be proposed.
* Approach: This paper proposed a second-order self-similar model to describe the network traffic. This paper also compare its model with existed model and analyze the meaning of its parameters in its model.
	* The differences between its model and original model are:
		* The variance(Covariance) of traffic is keep in high level even aggregate the data in long time period. In original model, the variance will come to 0 when aggregate data in long time(likes an hour).
		* The Hurst parameter H of the self-similar model denotes the "burstiness" of the network.
		* Contrary to intuition, Hurst parameter H increase with the total amount of traffic increase. This means that in larger networks, traffic tends to be turbulent rather than stable.
	* More information about Self-Similar Model in Math in Section 3
*  Benefit: Compare with former model, Self-Similar model denotes more characteristic about aggregation network traffic. This allows the network simulator to provide more realistic network data while making network protocol design more realistic.
*  Challenge: This paper miss several image to describe how this model fit the real-world network traffic. And the Hurst parameter H is a very specific number for different network, which need to be calculate case-by-case.
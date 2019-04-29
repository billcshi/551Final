# Note for Crovella97a

## NABC Overview
* Need: Several former paper have discussion about the self-similarity of the network traffic(see in Week8-Leland94a), this paper told about the self-similarity model in web traffic.
* Approach: The author modify the browser and recording the URL, transmission time and waiting time to analyze if it fit the self-similarity model and find the cause of the property.
* Benefit: This paper proposed the cause of the self-similarity in web traffic and it mostly due to the file traffic rather than the user.
* Challenge: This paper doesn't proposed how to deal with the self-similarity.

## Other Information

### Self-Similarity Model(Also cover in Leland94a)
1. Definition:
  * Given a zero-mean stationary time series X(X_1,X_2,...), we define the _m_-aggregated series X(m) by **summing** the original series X over **non-overlap** blocks of size _m_.
  * Then we say that X is _H_-self-similar, if **for all** positive _m_, X(m) has the same distribution as X rescaled by m^H
  * The same distribution means the _same autocorrelation_
2. Only 1 parameter in this model is H(Hurst parameter)
  * H denotes the degree of self-similarity
  * 1/2 < H < 1
  * As H->1, the degree of both self-similarity and long-range dependence increases
3. 4 Method to find the parameter H and proposed the degree of self-similarity
  1. Using the variance of X(m) against m on a log-log plot:
    * the slope=(-beta)
    * H = 1 - beta/2
  2. R/S plot: the rescaled range grows as a function of the number of points includes (n)
    * the slope of log-log plot of rescaled range against n is H
  3. periodogram method;
     * Spectral Density: Low frequency and High power traffic cause the long-range dependence and long period burstiness
  4. Whittle estimator:
    * The method 1-3 are graph method which lost some information about confidence intervals
    * The method 4 give the confidence intervals but need the form of the underlying stochastic process supplied.
    * This paper use fractional Gaussian noise(FGN) to calculate the H and confidence interval
  5. Self-Similarity Model fit well in middle range of time(seconds to tens of minutes), but not that well in ms-level(TCP connection) or day-level(Peek Hour pattern).

### Heavy-Tailed Distribution(Also cover in Labovitz10c)
1. Heavy-tailed distribution means the hyperbolic **asymptotic shape** of the distribution
2. Definition:
	* A distribution is heavy-tailed if:
	* P(X>x) ~ X^(-alpha) & x->INF & 0< alpha < 2
	* If alpha <= 2 then the variance is infinite
	* If alpha <= 1 then the mean is infinite
3. The Heavy-tailed distribution of traffic **is the cause of** Self-Similarity
4. In this paper:
	* use LLCD and HILL to create Heavy-Tailed Distribution Data

### Superimposing Heavy-Tailed Renewal Processes
1. A self-similar process may be constructed by superimposing(multiplexing, combination) many simple renewal reward processes
2. Definition:
  * Either the distribution of ON times is heavy-tailed with parameter alpha_1 or the OFF with alpha_2, or both
  * The sum is a self-similar fractional Gaussian noise process, with H=(3-min(alpha_1,alpha_2))/2
3. In this paper it prove:
  * In Transmission Time(ON Section):
    * The variance is very large(although it may not be infinite), which is similar to the heavy-tail property
    * the largest 10% packet count 50% of total traffic
  * The heavy-tail Transmission property has **nothing** to do with user behavior, only base on the heavy-tail of all **Available File**
    * The INF-Cache in Internet make every file only transmit once in the Internet.
    * And the Cache in NCSA is well behavior to simulate a INF-Cache.
    * No matter what user request, each file only transmit one (with 70% hit rate).
    * And the Available File is heavy-tail due to the magnitude different between text and video.(more than 10000 times)
  * The ON times are more likely responsible for the self-similarity than OFF time
    * The OFF time need to consider the OFF time cause by browser(CPU time) and the OFF time cause by USER(the IDLE time waiting for input)
    * The Heavy tail parameter of OFF time is bigger than ON time, which means it seems to have lighter tail than ON time
  * Using Pareto Distributing to fit Heavy-Tailed Traffic:
    * A cool thing is: the variance in Pareto Distributing is INF


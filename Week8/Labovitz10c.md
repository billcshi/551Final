# Note for Labovitz10c

## NABC Overview
* Need: Internet Inter-Domain structure changed in last few decades and in last 5 years(2005-2010) saw significant shift in Internet inter-domain traffic demands and peering police. This paper is needed to proposed these new inter-domain traffic characteristics.
* Approach: This paper used direct instrumentation of peering routers across multiple providers and monitor about 25% of all Internet traffic which is more than 200 EB over the two-year(2007-2009) study.
  * Measurement Methodology: Monitors in many ISPs network, Only had data from their customers(but they have many ISPs as customers, the coverage is not complete but it is probably large)
* Benefit: The crazy amount of data ensure the observations(list below) of this paper convincing. These observation can guide the designing of network protocol and help to solve other traffic engineering problem.
* Challenge: The data collection and processing done in this paper requires a lot of ISP support. And a lot of the information is the ISP's highly trade secrets. This experiment is difficult to repeat, and some of the features proposed in this paper are only for the Internet at a specific point in time.

## Internet Characteristics in 2010
1. hierarchy structure(former) vs. content provider peer with ISP
  * The former Internet structure is a hierarchy one.
  * With the growth of Giant content providers(both CDN e.g. Akamai and Content Provider e.g. Google), they directly peer-link to ISP(Make the network even more efficient).
  * These peering traffics grow faster than the other part of the Internet and take more percentage of total amount of traffic in inter-domain traffic.
2. Network traffic **Consolidation**:
  * Through long observations, the authors mentioned that network traffic is merging to **a small number** of ISPs and content providers
  * 150 top AS(over 30000 AS) takes more than 50% of total inter-domain traffic.
  * Google takes more than 5% of total traffic in 2009.
3. Web Application take more than 50% of total traffic compare with all other application
  * some application(e.g. Video) using Web application protocol to transform data
4. P2P traffic is decline most during this 2-year study.
  * More info and reason is propose in Section 4.4 
5. The total traffic volume measurement and annual growth rate(AGR) measurement method is proposed in Section 5:
  * The total volume is based on the linear fit of the Peak traffic data rate from each content provider with calculated weighted Avg percentage.
  * AGR is calculated by a MINTS method.
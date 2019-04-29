# Note For Shenker95a

## NABC Overview

* Need: Since the former architecture of the Internet is built for **data transmitted application(delay tolerance)**, and the majority of the application on the Internet is moved to the **multimedia application(delay sensitive)**, the basic architecture of the Internet should be discussed in nowadays phenomenon.
* Approach: This paper discusses whether the Internet should adapt new service models, how this service model should be invoked and whether this service model should include admission control.
  * This paper **provides a framework** for considering the various architectural **trade-offs** and identify **some of the critical assumptions** that lead to one choice or another.
    * In this paper, the author used **efficacy(V)** to describe the utility function of the network model. And discuss **each possible options** based on **efficacy function** 
* Benefit: In this paper, the author discuss every possible option for the above issues and predict the result of the various options. And make predictions Internet architecture.
* Challenge: When the paper is presented, the multimedia application was just started. So the author could only predict the possible situation of the multimedia application. With more application data nowadays, we can check the assumption and the result of this paper.

## The Important information of this paper

1. Current Internet Design:
   * **Best-effort** service: No admission control, no assurance about when and whether packets will be delivered.
   * **Traditional Data** application: They can tolerate packet delays and packet losses, so they are well served by best-effort service.
   * **Real-time** application: Typically less elastic, less tolerant of delay and losses variation, than data application.
     * Real-time application **do not perform adequately** because the variation of delay and packet dropped.
     * Real-time application do not back off in the presence of congestion. So the **tradition data application may be interfered** by the real-time application. 
   * **Solution1: Fair-queueing** can partially solve the second problem but has nothing to do with the first question.
     * Advantage of fair-queueing: It does not require any change to the Internet architecture.
   * **Solution2: Modify the Real-time application** to a more adaptive one:
     * Advantage: No change are required to any network interface. And the network mechanisms and application mechanisms are relatively well understood.
     * Challenge: The adaptability has limitation. And since there is no admission control, the fair share of bandwidth might be very small.
2. The goal of Network Design:
   * This paper use **Efficacy(V)** to evaluated whether the network satisfies the services:
     * From a basic question: how happy does this architecture make the **users**?
     * **Efficacy** equal to **the sum of each applications**.
   * The architecture design goal is to **optimize the efficacy**.
3. Why the network need **various service models**?
   * What is **service models**?
     * The interface(API) between the app and the network
   * **Compare** with using single best-effort service model using **various services** for different application **will increase** the efficacy.
     * A toy example of efficacy calculation is presented in Page 3 Chapter 4
   * **Matching services to application** enables the network to increase the overall utility(efficacy).
   * **No separate networks** for various application.
     * A toy example of efficacy calculation is presented in Page 4.
     * **Combining the network into a single infrastructure yields a much higher value** of efficacy.
   * **No every application** gains directly from the increase in efficacy:
     * The total bandwidth is not changed. If we allocate more bandwidth for high priority application, the lower application will decrease its utility.
     * The total utility of all applications will increase.
4. **How to match** services to application?
   * A good matching will increase the efficacy. However a bad matching will decrease the efficacy, even it may be lower than the best-effort model.
   * Implicitly Supplied: 
     * The applications do no change.
     * The **network** choose the service class for application based on port number, etc..
     * Advantage: Doesn't require any change to the service interface. And such architecture could be deployed immediately and then modified incrementally.
     * Disadvantage: 
       * This model only work with **a fixed set** of application classes.
       * This model cannot accommodate individual or situational variations **within a single application**.
       * This model has a **fairly basic architectural flaw**: the network is only made for existed application. The network design for the Internet should be allowed all kind of application.
   * Explicitly Supplied:
     * The applications asked for types of service they desire.
     * Advantage: 
       * Maintains the clean architectural interface between application and networks.
     * **Challenge1 Incentives**:
       * This approach need the application to choice the service. The network can achieve optimal efficacy **only if some applications ask for lower quality service**.
       * Based on the selfish behavior of humankinds, this requirement is hard to approach.
       * The author further discussed using pricing to deal with this issues in Page 6.
     * **Challenge2 lack of flexibility(Stability)**:
       * Application **must know the set of services** in order to ask for service.
       * **The services that are already in place cannot be altered** because it would interfere with the installed application base.
       * In order to **implement some outdated network services**, it may affect **the efficiency of the entire network**.
       * This approach also need service model to **be uniform**, which will hurt the subnet structure of IP network.
   * Link-Sharing and other implicit service:
     * This approach concerned with the service given to an aggregate of flows.
       * For example, the university fairly divided its bandwidth to each school.
     * This kind of service is indeed implicitly supplied. However, it has little to do with the problems studied in this paper.
5. Do we need admission control?
   * The definition of **Overload**:
     * If we **add more applications** into the network the total **efficacy is decreasing** than the network is overload.
     * Overload may lead to **congestion collapse**. This problem actually happened... see the [Dhamdere18a] (Week4) paper(Persistency congestion pattern in interdomain links).
   * Some network will never overload and some will overload after a number **N** of application.
   * How to find **N**?
     * The calculation process is presented in Page 8.
     * In traditional data application network, N equal to **infinity**, so the network will never overload.
     * In real-time application network, the network will overload after a number of users.
   * So we need **an admission control to reject more users** after the network entered the overload point.
   * Why **don't we overprovisioning** the network?
     * Both in short term and long term, overprovisioning the network is impossible.
     * In short term, the Internet growth so fast.
     * In long term, the problem of who need to pay for overprovisioning cannot be solved.
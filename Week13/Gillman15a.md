# Note For Gillman15a

## NABC Overview

* Need: An approach is needed to deal with website attack.

  * The attack time and volume are increasing fast in last several years.
  * **Volumetric Attack**(DDOS Attack)
    * **SYN Flood**:
      * Attacker **send a number of SYN packets** to a webserver.
      * Attacker will not reply to the server's SYN-ACK packets with ACK packets.
      * Webserver need to maintain these "half-open" TCP connection in its memory.
      * Since its memory is limited, **the legitimate users' TCP connection might be drop**
    * **UDP Flood**:
      * UDP is connectionless and doesn't require an initial handshake between the client and server.
      * Attackers **use spoofed IP source address** to **avoid firewall filtering**.
      * Attackers also **randomize the port** to **avoid port-filtering firewall**.
    * Application-layer Attack:
      * Using **DNS flood**.
        * When the name servers' resources are exhausted, **legitimate users cannot receive valid DNS responses**.
    * Large-scale volumetric attack often present **multiple types of floods simultaneously**.
    * Such attacks typically employ **botnets** of personal computers and servers infected with malware.
  * **Reflection/Amplification Attack**
    * The volumetric attack need **an attacker must have more resources than the target**.
    * Reflection and Amplification can **make a larger volume attack** to the target server.
      * A distributed reflected DoS(DRDoS) attack could enable a perpetrator to direct **more than an order of magnitude more traffic** at a target than the attacker directly generates.
    * A **common DRDoS attack process**:
      * The attacker sends numerous name resolution requests to recursive name servers(**DNS**) around the world.
      * The attacker place the **target's IP address** in **DNS request packets**.
      * So the target server **will receive DNS reply packet** from DNS servers.
      * The DNS **request packet is only 64bytes**, but the DNS **reply packet is over 3 kilobytes**. So the attackers volume is amplify 50 times to the target server.

  * **Web application exploits**
    * The attacker may not only deny the servers' application, but also get the private data from the servers.
    * **SQL injection attacks**: using SQL query to get security data from target database.
      * These attack is easily prevented by type-checking the input.
    * **XSS attacks**: Attacker introducing a malicious script(JavaScript code) into a dynamically generated webpage on a trusted website.

* Approach: **Secure Delivery Network**

  * Network architecture:
    * DNS Servers: For providing distributed DNS resolver around the world and defend the DNS flood attack.
    * Edge Servers: These servers act as a distributed firewall to stop DDoS attacks and data theft at the network edge.
    * Overlay network: Connecting original server to edge servers.
    * Origin: Operated by the content or server provider to provide origin data of web.(User's own server)
  * DNS protection:
    * Since the Akamai's DNS system is large enough, the DNS flood attack cannot overload the DNS system.
    * It also filters out potentially malicious DNS requests at the earliest possible instance.
  * EDGE protection:
    * Using filtering rule to locate the malicious packet in Edge Servers.
      * Network-layer control: IP address filtering.
      * Adaptive rate control: **Classify clients** according to their request pattern and can filter or rate-limit requests based on this classification.
      * Application-layer control: Inspect the header and body of HTTP requests to defend SQL injection and XSS attacks.
      * Client reputation rules: **Classify clients** according to **its historical action** in whole Akamai's system.
    * Providing alters and drop the malicious packet.
  * Origin server protection:
    * Using caching in the edge server to reduce the traffic to the original server.
    * The origin server only connect to the edge server rather than the outside clients.
    * Using a better caching configuration to defend **cache-busting attack**.
      * **Cache-busting attack**: Forcing a large volume of attack requests to be send back to the origin server.
  * WAF: Web Application Firewall
    * checks incoming web requests
    * tries to filter unsafe things, for examples SQL injection

* Benefit: Using a case study to show how Secure Delivery Network can deal with different kinds of attack.

  * Akamai can deal with different kinds of attack which mentions above.
  * The average volume in Akamai's network is several TB per second and the peck volume of attacker is hundreds of GB per second, only 1 twentieth of its system. So the DDoS can hardly work in Akamai's system.

* Challenge: Still some attack can not be detected by its system, so the attack detection still need to improve.
# Note For Dingledine04a

## NABC Overview

* Need: An improvement application of Onion Routing(OR)
  * The problem of original OR
    * OR provide a protocol to help clients to build a **circuit network**, each now in the path **only know its predecessor and successor**.
    * But OR design and **deployment** issues were **never resolved** and the design has not been update.
    * An single OR circle can only use **for one TCP connection**.
  * The **design Goal** of Tor
    * **Deploy ability**: Since the original OR cannot be deployed in the real world. The first goal of Tor is deployable.
      * Tor must not be expensive to run.
      * Tor must not place a heavy liability burden on operators
      * Tor must not be difficult or expensive to implement
      * Since Tor is used in **real-world situation**, Tor should be able to **deal with congestion**.
    * **Usability**: The latency of Tor should be reasonably low.
      * Also require as few configuration decisions as possible.
      * Also Tor should be able to implementable on all common platforms.
      * Robust to any single failure.
    * **Flexibility**: Tor can serve as a test-bed for future research.
    * **Simple Design**: The protocol's design should be well-understood.
  * A cool thing of this paper is it provide what is **Non-Goal**:
    * Non peer-to-peer: Tor system separate **user client**(OP, **Onion Proxy**) and **volunteering router**(OR,**Onion Router**)
    * Not secure against end-to-end attack: Not claim to solve end-to-end timing or intersection attacks.
      * Also don't deal with **traffic-confirmation attack**.
      * Tor assume **attackers are limited**(cannot get all nodes or all traffic).
    *  No protocol normalization
    * Not steganographic: Not try to conceal who is connected to the network. **But it conceal which two is connected.**
  * Threat Model: Listed in **Section 7**, also provide solution for these attack
* Approach: Tor provide an improvement of original Onion Routing.
  * **Anonymity** definition in **Tor**:
    * Hiding the source IP address
    * Confidentiality of the content except to the final destination
  * **Cells**(Tor's Packet)
    * Traffic passes along these connection in **fixed-size cells(512 bytes)**.
    * 512 bytes includes a **header** and **payload**
      * Header: include a **circuit identifier (circID)** and a **control cell commands (CMD).**
  * **Relay cells**(cell for relay message):
    * Include **cell header** and **relay header** and **payload**
    * Cell header is same
    * For Relay Header:
      * streamID(support multiple TCP streaming over a circuits network)
      * Digest(For end-to-end checksum)
      * Length of the relay payload
      * Relay command
  * How to start a steam:(in Section 4.2)
    * Build a 2-hops circuit:
      * A send create cell and key(a new random key encrypted by B's public key) to B
      * B return its negotiated key to A
    * Build a 3-hops circuits:
      * A first build 2-hops circuit to B
      * A asks B to extend circuit to C by relay cell
      * B build 2-hops circuit to C with a new circuit id
      * B return the relay cell to A
      * A picks new circuit key(encrypted by C's public key) to C via B
    * Only A know the topologic of the whole circuit, the other node only know the successor and predecessor of itself.
  * A client can open and close a circuit it created
  * Integrity checking only on end-to-end
  * Deal with congestion:
    * Client can do rate limiting using token method to control how much cell can pass through the server
    * Congestion control both on circuit-level and stream-level by control the number of cell.
      * Note that each cell have a fix-sized.
  * **Anonymity of the Server(by rendezvous point)**:
    * Server can advertise to its **introduction point**.
    * Client can set up its **rendezvous point** in a remote server by Tor.
    * Then Client **connects to one introduction point** to **informs the Server about its rendezvous point**.
      * Client get the information about the Server **by out-of-bound method.**
      * Client provide not only its rendezvous point but also other handshake information.
    * Then the **server will connects to the rendezvous point** by Tor.
    * The **rendezvous point connects Client's circuit to Server's circuit**.
* Benefit: The author deploy a 32-nodes Tor system and run well to defend all kinds of attack mention in his paper.
* Challenge: The Tor still introduce a high latency to the network.

## Note in Class

1. Key Ideas:
   * goal: anonymous communication
     * hide the IP address of an internet connection
     * and the server they talk to
     * and the content
   * Onion routing:
     * encrypt all communication:
       * content is not changed
       * hides the contents
     * Wrap traffic in layer and send it through a proxy
       * proxy: hide the IP address of the originator
     * We have multiple layers
       * so compromise any single site doesn't break anonymity
   * Why a new onion router?
     * new features: congestion control, perfect forward security, etc.
     * Most important: Wanted an actually usable system(**deployable**)
2. Goal and Non-Goal of Tor
   * Fundamental Goal: Anonymity, hide the sources IP address
   * Goal: robust to any single failure
   * Goal: usable, reasonably low delay, flexibility, can run on normal machines
   * Goal: cope with real-world traffic and congestion
   * Non-Goal: assume attackers are limited(cannot get all nodes or all traffic)
   * Non-Goal: don't plan to stop timing attacks
3. Defining "anonymity" in Tor
   * hide your IP address
   * and confidentiality of the content(except to the final destination)
   * the user has to cooperate
     * hiding cookies and user names and other kinds of signatures
   * Backward Anonymity in Tor: Using random OID
   * Forward Anonymity in Tor: Using key to encrypt for each hop
4. **Parts(Mechanisms) of Tor** (Alice send to Bob):
   * onion proxy(OP): the first hop
     * get clear text traffic from Alice
     * so hopefully it's on Alice's computer(but not always)
   * onion routers(OR): intermediate hops
     * each does wrapping and unwrapping
   * exit router(ER): last hop
     * it **get clear text** out and sends it to the actual destination
   * circuits: path through ORs
   * streams: something with a stream id and a real-world destination, for each TCP connection
5. Rendezvous Point:
   * Goal: the server's location is anonymous(Hide Bob's IP address)
   * Mechanisms:
     * need to identify where you want to go:
       * name of the service is a big hash
       * find the hash  value by some out-of-band ethod
     * given the hash:
       * want to build a Tor path to it
       * the hash identifies some node in the Tor system
       * the server builds a Tor connection to the rendezvous point
       * the rendezvous point passes traffic between the two connections
     * how well does it work?
       * keeps anonymity but both can talk
6. The improvement of Tor:(Listed in Section 1)
   - Perfect forward secrecy
     - In original OR, a single hostile node could record traffic and later compromise successive nodes in the circuit and force them to decrypt it.
     - Torr uses an incremental or **telescoping** path-building design.
     - Once these keys are deleted, subsequently compromised nodes cannot decrypt old traffic.
   - Separation of "protocol cleaning" from anonymity:
   - No mixing, padding, or traffic shaping
   - Many TCP streams can share one circuit
   - Leaky-pipe circuit topology: Tor initiators can direct traffic to nodes **partway down the circuit**.
   - **Congestion** control: Uses end-to-end acks to maintain anonymity and detect congestion.
   - Directory servers: *Trusted nodes act as directory servers*.
   - Variable exit policies: Each operator is comfortable with **allowing different types of traffic to exit** from his node.
   - End-to-end integrity checking: Tor **verifying data integrity** before it leaves the network.
   - Rendezvous points and hidden services
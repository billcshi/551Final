# Note for Bosshart14a

## NABC Overview

* Need: A general, extensible approach would be simpler, more elegant, and more future-proof to represent the SDN rules for OpenFlow standard.
  * The goal of P4 approach:
    * Reconfigurability: the controller should be able to redefine the packet parsing and processing in the field.
    * Protocol Independency: can deal with all kind of protocol header
      * The OpenFlow can only deal with specific protocol
    * Target Independency: **a compiler** should compile P4 to specific machine language for different switches
  * The abstract forwarding model(Figure 2 in the paper):
    * An input interface to deal with input packet
    * A **parser** to traverse through the input packet header and modify the header when needed
    * A Match+Action table to deal with the ingress pipeline and store to buffer.(Ingress process determines the egress port and the queue into which the packet is placed)
    * Another Match+Action table to deal with egress pipeline from buffer to output interface.(Egress process modifications to the packet header)
    * Two types of operations:
      * Configure: to program the parser, the process stages
      * Populate: to add new entry to Match+Action table
    * Packet can carry additional information between stages, called **metadata**
  * Why we need a new programming language:
    * We need to express how a switch is to be configured and how packets are to be processed.
    * The former language have some shortcoming:
      * Click: insufficient to mirror the parse-match-action pipelines in dedicated hardwares
      * TDG: not readily accessible to most programmers
    * **Two step** compilation process for P4:
      * programmers express packet processing programming using P44
      * compiler translates the P4 to TDGs to facilitate dependency analysis
* Approach: This paper present the P4 language and give a toy example of using this language.
  * The parser is a state machine to deal with different header
  * Compiler translate P4 according to different underlying structure. 
* Benefit: P4 is very simple to express the rule of SDN. It also possible to deal with reconfiguration the switches network during processing.
* Challenge: P4 is a new language for programmer to use.

## Note during course

1. Successes and Challenges:
   * OpenFlow work well for a lot of things:
     * central controller makes the decisions
     * fairly simple abstraction that people built a lot of hardware against
     * opened up networking with a vendor-independent API
# Note For Clark02a

## Overview
1. internet landscape now
2. technical design principles for tussle environment of network
3. tussle space and related design
4. revisiting of some old network principles

## Internet Landscape
1. parts of the Internet: Users, ISP, Private sector network providers, Governments, Intellectual property right holders, Providers of content and higher level services
2. tussles span a broad scope

## Design Principle
1. Design for variation in outcome
2. Modularize:
	* Functions that are within a tussle space should be logically separated from functions outside of that space
3. Design for choice:
	* Protocols must permit all the parties to express choice.
4. Implications:
	* Open interface
	* Tussles often happen across interface
	* It matters if the **consequence** of choice is visible(The choice could be secret or not)
	* Tussles have different flavors
	* Tussles evolve over time
	* There is no such thing as **value-neutral design**.
	* You design the playing field, not the outcome.

## Tussle Space
1. Economics:
	* Our principle that one should design choice into mechanism is the building block of competition.
	* Provider lock-in from IP addressing
		* Tussle between addresses reflect topology to support efficient routing and desire of the customer to change providers easily
		* Address should reflect connectivity, not identity, to modularize tussle
	* Value pricing
		* due to there is no value-neutral design
		* User want to sidestep providers restriction of internet used.
	* Residential broadband access
		* the ISP in broadband genneration must be far less than the ISP in dialup gen
		* modularized along tussle space boundaries
	* Competitive wide area access
		* user want to choose which long distance provider to use
2. Trust:
	* The implies of firewall is opposite the basic idea of Internet(which is transparent packet carriage)
	* The role of identity and anonymously
3. Openness:(open interface and Vertical integration)

## Old principle
1. End to end arguments:
	* Still right for innovation and reliability but need a more complex articulation
	* Peeking is irresistible
2. Separation of policy and mechanism:
	* There is no value-neutral design
	* User Empowerment(User have the right to choose)
	* Doing nothing more

## Conclusion
1. Design a new enhancement for the Internet should analyze the tussles that it will trigger.
2. We(technical Designers) should not try to deny the reality of the tussle, but instead recognize our power to shape it
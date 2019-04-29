# Note For Goldberg14a

## Overview
1. Common BGP policy
2. Several Attacks types on BGP
	* prefix hijacking
	* route leaks
3. Defense Methods For Attacks:
	* prefix filtering
	* RPKI
	* BGPSEC
4. Sometimes fixes(Defense method) need to be deployed widely
	* by people who don't directly benefit

## Common BGP policy
1. IP prefixes
2. Longest-prefix-match Routing
3. Autonomous System
4. Business relationships and Routing policies
	* customer-provider relationship
	* settlement-free peering relationship
	* AS always avoid forwarding if it cannot generate revenue
	* rule of thumb: AS a will announce a route to AS b only if 
		1. b is a customer of a
		2. the route is for a prefix originated by a
		3. the route is through a customer of a

## Attacking method on BGP

### Hijacks(2 types of hijack)
* prefix hijacks
	* hijack the entire prefix
	* split the network traffic, people choose one route or other
* subprefix hijacks:
	* based on Longest-prefix-match Routing policy
	* hijack all traffic to subprefix
* detecting hijacks attack

### Route Leaks
* providing announce violating the rule of thumb
	* maybe from customer to provider
	* maybe to its peers
* based on AS can generate more revenue by forwarding traffic through its customer
* the announcer get a lot more traffic than they expect

### Impact of routing incidents
* blackholes: drop all traffic enter the router
* interception: 
	* interception is invisible to end users
	* the bogus AS will transit the traffic back to the legitimate origin AS

## Defenses method

### Prefix filtering
1. using whitelisting technique to filter the announces from each of its customers
2. benefit:
	* simple and effective
	* make sure wrong people don't announce stuff
3. challenge:
	* works only on customer links, hard for peers
	* Lopsided incentives: the AS other than the provider itself can do nothing using this method

### RPKI
1. RPKI: provide a trusted mapping table from IP prefixes to original AS
	* central authority gives each AS either prefixes and public key
	* ASes sign their prefixes and announce that(a ROA: Route Origin Authorization)
2. benefit:
	* Offline cryptography
	* Protection from hijacks
	* Effective incentives
	* prevent other ASes from announcing a given prefix
3. challenge:
	* RPKI takedowns and misconfigurations: attacker may attack RPKI itself
	* RPKI can be circumvented(Including route leak and path-shortening attack)
	* Need to trust central authority(RIRs)

### BGPSEC(Path validation):
1. path validation based on BGPSEC signature:
	* the prefix and AS-level path
	* the AS number of the AS **receiving** the BGPSEC message (the message provider need to signed its receiver)
	* all the signed messages received from the previous ASes on the path
2. benefit:
	* No path-shortening attacks
	* all BGP steps are verified
3. challenge:
	* not clear this protects against route leaks
	* online cryptography: high computational overhead
	* The transition to BGPSEC: The method is useful only if **all** ASes on the path have applied BGPSEC
	* ASes have deployed BGPSEC can still suffer from **protocol downgrade attacks** if some ASes don't use BGPSEC

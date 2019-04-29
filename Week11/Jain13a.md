# Note For Jain13a

## Note in class

1. Key Ideas:
   * B4, the WAN between google's datacenter
   * a whole bunch of stuff:
     * switches, etc.
     * two kind of traffic:
       * elastic traffic: large changes in amount, can tolerate loss
       * high priority traffic(user data)
     * control over the network
   * goal:
     * good utilization
     * need to cope with hardware failures
     * traffic engineering(load balancing)
2. Goals and Assumptions:
   * Goal:
     * Want very high utilization(near 100%)
       * prior work where WAN links often ran at <50%
     * Want to support prioritized traffic(Some urgent, much bulk)
     * Separate hardware and software
       * use commodity hardware
       * want to allow software to evolve quickly
   * Assumptions:
     * Failures are inevitable
     * Have a lot of elastic traffic
     * We can share bandwidth across traffic
     * They control everything:
       * applications
       * switches in datacenter
       * links and circuits between mechines
3. Approach:
   * global: traffic engineering(TE)
     * done centrally(not that many datacenter)
   * per-datacenter(DC):
     * OFC(open flow controllers), Quagga(redundant routing)
     * OFC compare to Ethane(course project):
       * on failure, Ethane restart like soft state
       * Google replicated controlled so can recover quickly
     * why per DC? want datacenter keep running even if WAN fails
   * inside DC:
     * OF switches(OFA)
     * apps
     * why OF here? want central controller
# Data Marshalling

1. How do computers communicate?
   * sockets
   * messages, network protocols
2. What makes protocols hard?
   * computers are different
     * 32-bit and 64-bit computer architecture
     * different generation of hardware and software
   * different protocols
     * IpV4 and IpV6
     * TLS 1.0 vs 1.2 vs 1.3
3. What are the problems in exchanging data? 
   * **Endianness**.
     * Big-endian: PowerPC
     * Little-endian: x86, ARM
   * Data Structure Padding
   * Some Partial Solutions:
     * RFCs: ASCII art diagrams that lay out exact structure sizes
     * class project: use htons to put things in network byte order
     * today in the web: JSON, XML, YAML
       * these support complex data types: lists, sets, etc.
       * because they're tree structure and have good libraries
       * they're just **text**
         * good: flexible and easy to extend
         * bad: collisions, and text is large and slows
     * send with HTTP GET/PUT
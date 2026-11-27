# MPLS-ISIS-LAB--01

This is a 6-router MPLS core running IS-IS as the IGP, with BFD on the links for fast failure detection. All links are /30 point-to-point connections on GigabitEthernet interfaces, and each router has a /32 loopback, which is typically used as the router ID, LDP/MPLS identifier and IS-IS/BGP source.</rb>

Routers and loopbacks</br>
Router	Loopback0</br>
r-mpls-1	172.1.1.1/32</br>
r-mpls-2	172.2.1.1/32</br>
r-mpls-3	172.3.1.1/32</br>
r-mpls-4	172.4.1.1/32</br>
r-mpls-5	172.5.1.1/32</br>
r-mpls-6	172.6.1.1/32</br>

Links</br>
Subnet	    Endpoint A	  Endpoint B</br>
10.1.0.4/30	r1 Gi0/0 (.6)	r2 Gi0/0 (.5)</br>
10.2.0.0/30	r2 Gi0/1 (.2)	r3 Gi0/0 (.1)</br>
10.3.0.0/30	r3 Gi0/1 (.1)	r4 Gi0/0 (.2)</br>
10.4.0.0/30	r4 Gi0/1 (.1)	r5 Gi0/0 (.2)</br>
10.5.0.0/30	r5 Gi0/1 (.1)	r6 Gi0/0 (.2)</br>
10.6.0.0/30	r1 Gi0/1 (.1)	r6 Gi0/1 (.2)</br>
10.7.0.0/30	r1 Gi0/2 (.1)	r5 Gi0/2 (.2)</br>
10.8.0.0/30	r2 Gi0/2 (.2)	r4 Gi0/2 (.1)</br>
10.9.0.0/30	r2 Gi0/3	r5 Gi0/3</br>

Topology shape</br>

Outer ring: r1 → r2 → r3 → r4 → r5 → r6 → r1.</br>
Cross-links (chords): r1–r5, r2–r4 and r2–r5.</br>
Role of each router</br>
r-mpls-2 and r-mpls-5 are the most connected, with 4 links each (r2: r1, r3, r4, r5; r5: r4, r6, r1, r2). They act as the core hubs.</br>
r-mpls-1 and r-mpls-4 each have 3 links.</br>
r-mpls-3 and r-mpls-6 each have only 2 links, so they are the least redundant nodes, effectively sitting on the ring edge.</br>

Observations</br>

Redundancy: every router has at least two paths, so no single link failure partitions the network. r3 and r6 lose their path diversity if one of their two links fails.</br>
Missing labels: the 10.9.0.0/30 link (r2 Gi0/3 to r5 Gi0/3) has no host addresses shown. Presumably .1/.2, but worth documenting.</br>
BFD and IS-IS: BFD is typically enabe</br>
led per interface (isis bfd) so that sub-second failures trigger IS-IS reconvergence and LDP/label updates quickly. Make sure timers (for example 300 ms × 3) are consistent on both ends of each link.</r>
MPLS: confirm mpls ip is enabled on all core interfaces and that LDP router-IDs use the loopbacks. Check that MTU accommodates label overhead (mpls mtu or a larger interface MTU).</r>
Loopback reachability: IS-IS should advertise all six loopbacks, and LDP only needs to bind labels for those /32s.</r>

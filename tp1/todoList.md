
### 1\. Topology and configuration

- [X] Screenshot the complete GNS3 topology with router names, interfaces, subnets, and AS numbers.
- [X] Document all router interface addresses, loopbacks, PC addresses, masks, and gateways.
- [X] Resolve or explain the differences from the assignment: `20.0.x.0/30` internal OSPF links, `30.0.36.0/30` between R3–R6, and the two `/25` LANs on R5/R6.
- [ ] Save `show running-config` from every router.
- [ ] Check `show ip interface brief`: all required interfaces have the correct addresses and are up.
- [ ] Check interface speed and duplex using `show interfaces`.
- [ ] Verify each PC can ping its gateway.

### 2\. EIGRP — R1, R2, R3

- [ ] Verify EIGRP AS 65001 and the advertised networks using `show ip protocols`.
- [ ] Verify both expected neighbors on each router with `show ip eigrp neighbors`.
- [ ] Save `show ip route` and `show ip eigrp topology`.
- [ ] For representative LAN and loopback routes, record destination, route source, next hop, outgoing interface, metric, and administrative distance.
- [ ] Identify internal routes (`D`) and redistributed external routes (`D EX`).
- [ ] Explain the metric configuration used when redistributing BGP into EIGRP.
- [ ] Identify successors and any feasible successors shown in the topology table.

**Capture on an internal EIGRP link:**

- [ ] Periodic Hello messages.
- [ ] Neighbor establishment and initial Update exchange.
- [ ] Acknowledgments where present.
- [ ] Messages during a link failure and recovery: Updates, Queries, and Replies where they occur.
- [ ] Inspect advertised prefixes and metric fields.

Wireshark display filter: `eigrp`

### 3\. OSPF — R4, R5, R6

- [ ] Verify area 0, advertised networks, and router IDs using `show ip protocols` and `show ip ospf`.
- [ ] Save `show ip ospf neighbor`; verify the expected neighbors and adjacency states.
- [ ] Save `show ip ospf interface` to document costs, timers, network types, and DR/BDR roles.
- [ ] Save `show ip route` and `show ip ospf database`.
- [ ] For representative routes, record next hop, outgoing interface, cost, and administrative distance.
- [ ] Identify internal routes (`O`) and redistributed external routes (`O E1` or `O E2`).
- [ ] Document the external metric type and metric used for BGP redistribution.

**Capture on an internal OSPF link:**

- [ ] Hello messages, including area, router ID, timers, and neighbor list.
- [ ] Adjacency establishment: Database Description, Link State Request, Link State Update, and Link State Acknowledgment packets.
- [ ] Router and network LSAs where present.
- [ ] External LSAs carrying redistributed prefixes.
- [ ] LSA changes and flooding during link failure and recovery.

Wireshark display filter: `ospf`

### 4\. BGP — R3 and R6

- [ ] Save `show ip bgp summary` on both routers and verify the session is established.
- [ ] Save `show ip bgp neighbors` to document peer addresses, AS numbers, and negotiated timers.
- [ ] Save `show ip bgp` and `show ip route bgp`.
- [ ] Inspect representative prefixes with `show ip bgp <prefix>`.
- [ ] Record prefix, next hop, AS-PATH, origin, MED, and other attributes shown.
- [ ] Explain which routes are locally originated and which are learned from the other AS.

**Capture on R3–R6:**

- [ ] TCP connection establishment on port 179.
- [ ] BGP OPEN messages: AS numbers, hold time, router IDs, and capabilities.
- [ ] KEEPALIVE messages.
- [ ] Initial UPDATE messages containing announced prefixes, next hops, and path attributes.
- [ ] Subsequent UPDATE messages during an internal failure, if advertisements change.
- [ ] Withdrawals when a previously advertised prefix becomes unreachable.
- [ ] Session reestablishment if you perform a separate BGP restart test.

Wireshark display filter: `tcp.port == 179`

Start the capture **before** bringing up or restarting the session to record the handshake and initial exchange.

### 5\. Redistribution

- [ ] Document the redistribution commands on R3 and R6, including connected routes.
- [ ] Trace an R1 LAN prefix through **EIGRP → R3 BGP → R6 BGP → OSPF → R4/R5**.
- [ ] Trace an R4 LAN prefix through **OSPF → R6 BGP → R3 BGP → EIGRP → R1/R2**.
- [ ] For each stage, save the route entry and explain changes in route source, metric, administrative distance, next hop, and BGP attributes.
- [ ] Check whether LANs, loopbacks, and transit networks are advertised as intended.
- [ ] Check for unexpected prefixes or routes being redistributed back into their originating domain.

**Capture evidence:**

- [ ] BGP UPDATE advertising the selected prefixes.
- [ ] EIGRP external route advertisement for an OSPF-side prefix.
- [ ] OSPF external LSA for an EIGRP-side prefix.

### 6\. End-to-end connectivity

- [ ] Test all 15 unique PC pairs with ping; record results in a table.
- [ ] Check reverse-direction connectivity, particularly across the AS boundary.
- [ ] Save representative traceroutes within each AS and between ASes.
- [ ] Explain the observed paths using the routers’ routing tables.
- [ ] Test reachability of the loopbacks if included in the routing design.

**Capture during a cross-AS ping:**

- [ ] ICMP Echo Requests and Echo Replies on R3–R6.
- [ ] ARP request/reply on a LAN when resolving the gateway.
- [ ] Traceroute probes and ICMP responses, if used as report evidence.

Wireshark display filters: `icmp` and `arp`

### 7\. Failure and convergence tests

Repeat the following separately for **one internal EIGRP link** and **one internal OSPF link**, choosing a link used by the traffic under test:

- [ ] Record the initial route, next hop, traceroute, and neighbor state.
- [ ] Start a continuous ping across the affected path.
- [ ] Start protocol captures on surviving links where routing changes will be exchanged.
- [ ] Record the time and method of failure: interface shutdown or link suspension.
- [ ] Save the changed neighbor and routing tables.
- [ ] Record the replacement path and verify it with traceroute.
- [ ] Record lost packets and the time until successful replies resume.
- [ ] Use capture timestamps or logs to describe routing reconvergence separately from observed ping interruption.
- [ ] Restore the link and capture recovery.
- [ ] Explain the differences observed between EIGRP and OSPF.
- [ ] Check whether the R3–R6 BGP session remained established and whether advertised prefixes changed.

A redundant internal-link failure may produce **no BGP withdrawal** if the prefixes remain reachable. Record that result.

### 8\. Report and submission

- [ ] Include topology and addressing tables.
- [ ] Include relevant configuration excerpts.
- [ ] Include annotated routing tables and explain metrics and administrative distances.
- [ ] Explain redistribution using the two traced prefixes.
- [ ] Include the connectivity results and representative traceroutes.
- [ ] Include before/during/after evidence for both failure tests.
- [ ] Include annotated Wireshark screenshots identifying packet types and relevant fields.
- [ ] Compare EIGRP, OSPF, and BGP using your observed results.
- [ ] Save the `.pcapng` files so screenshots can be traced to their original captures.
- [ ] Submit the GNS3 project, router configurations, and report to both teachers.

For efficient capture collection, use five sessions: **EIGRP startup, OSPF startup, BGP startup, EIGRP failure/recovery, and OSPF failure/recovery**, plus a short LAN/cross-AS connectivity capture.

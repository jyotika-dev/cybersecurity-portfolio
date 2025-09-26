# 🐧 LinuxJourney - Routing

This file summarizes my learning from **LinuxJourney** on network routing concepts and protocols.  
https://labex.io/lesson/what-is-a-router  
I practiced these on my local terminal and network environments.

---

## 🔹 What is a router?
- **Network device** → forwards packets between different networks.
- **Layer 3 operation** → makes decisions based on IP addresses.
- **Packet forwarding** → determines best path to destination.
- **Network interconnection** → connects LANs, WANs, and internet.
- `ip route show` → view local routing decisions.
- **Default gateway** → router that handles unknown destinations.

## 🔹 Routing Table
- **Database** → contains network destinations and paths.
- **Destination network** → target IP address/subnet.
- **Next hop** → IP address of next router in path.
- **Interface** → outgoing network interface.
- **Metric** → cost/preference value for route selection.
- `route -n` → display routing table in numeric format.
- **Static routes** → manually configured entries.

## 🔹 Path of a Packet
- **Source to destination** → step-by-step packet journey.
- **Routing decision** → made at each router hop.
- **TTL decrement** → prevents infinite loops.
- **Next hop lookup** → consult routing table for forwarding.
- `traceroute destination` → trace packet path through network.
- **Hop-by-hop forwarding** → each router makes independent decision.

## 🔹 Routing Protocols
- **Dynamic routing** → automatic route discovery and updates.
- **Interior Gateway Protocols (IGP)** → within autonomous systems.
- **Exterior Gateway Protocols (EGP)** → between autonomous systems.
- **Convergence** → time to update all routing tables.
- **Administrative distance** → trustworthiness of routing source.
- **Metrics** → cost calculation for path selection.

## 🔹 Distance Vector Protocols
- **Bellman-Ford algorithm** → calculates shortest path.
- **Hop count metric** → number of routers to destination.
- **Periodic updates** → broadcast entire routing table.
- **Count to infinity** → potential loop problem.
- **RIP (Routing Information Protocol)** → classic distance vector.
- **Split horizon** → prevents routing loops.

## 🔹 Link State Protocols
- **Dijkstra algorithm** → shortest path first calculation.
- **Link State Database** → complete network topology view.
- **LSA flooding** → share link state advertisements.
- **Faster convergence** → compared to distance vector.
- **OSPF (Open Shortest Path First)** → common link state protocol.
- **Area concept** → hierarchical network design.

## 🔹 Border Gateway Protocol
- **BGP** → internet backbone routing protocol.
- **Path vector protocol** → prevents loops with AS path.
- **Policy-based routing** → business relationships influence paths.
- **Autonomous System (AS)** → collection of networks under single control.
- **eBGP vs iBGP** → external vs internal BGP sessions.
- **Internet routing** → how packets traverse the global internet.

---

📌 **Applied Insight**:  
Routing knowledge is essential in cybersecurity:
- **Network reconnaissance** → understanding routing helps map network topology.  
- **Traffic analysis** → routing patterns reveal network architecture and data flows.  
- **BGP hijacking** → attackers can manipulate internet routing for traffic interception.  
- **Network segmentation** → proper routing design limits lateral movement.  
- **Incident response** → routing logs help trace attack paths and data exfiltration routes.

# Border-Gateway-Protocol-BGP-Network-Simulation

This project demonstrates the configuration and simulation of Border Gateway Protocol (BGP) using Cisco Packet Tracer. The goal is to show how BGP enables communication between multiple autonomous systems (AS), simulating how Internet Service Providers (ISPs) and large enterprises exchange routing information globally.

The topology consists of three autonomous systems (AS) connected via BGP:

#### AS20 → Represents one enterprise network.

#### AS35 → Acts as a transit or ISP network between the two enterprises.

#### AS50 → Represents another enterprise network.

Each AS has its own internal LAN, routers, and connected hosts, and BGP is used to exchange routes between them.

## Network Topology Overview
<img width="1902" height="725" alt="BGP topology" src="https://github.com/user-attachments/assets/63a15b6a-7e6d-49cf-9b3b-1180abc6a618" />

### Autonomous Systems:

#### AS20: Local LAN 192.168.10.0/24 connected to Router1.

#### AS35: Transit/ISP network connecting AS20 and AS50 via Router0.

#### AS50: Local LAN 192.168.20.0/24 connected to Router2.

### Router Roles:

#### Router1 (AS20): Originates local network 192.168.10.0/24 and establishes eBGP peering with Router0 (AS35).

#### Router0 (AS35): Core transit router that peers with both Router1 (AS20) and Router2 (AS50). This router facilitates route exchange between the two AS networks.

#### Router2 (AS50): Originates local network 192.168.20.0/24 and establishes eBGP peering with Router0 (AS35).

## Implementation Steps
### IP Addressing and Interface Configuration

Assign IP addresses according to the topology diagram (BGP topology.png). Example for Router1 (AS20):
```
hostname Router1
interface g0/0
ip address 10.10.10.2 255.255.255.252
no shutdown
interface g0/1
ip address 192.168.10.1 255.255.255.0
no shutdown

```
Repeat similar steps for all routers using their corresponding IP plan.

### Configure BGP on Each Router
#### Router1 (AS20)
```
router bgp 20
bgp router-id 1.1.1.1
neighbor 10.10.10.1 remote-as 35
network 192.168.10.0 mask 255.255.255.0

```

#### Router0 (AS35)
```
router bgp 35
bgp router-id 2.2.2.2
neighbor 10.10.10.2 remote-as 20
neighbor 20.20.20.2 remote-as 50
network 10.10.10.0 mask 255.255.255.252
network 20.20.20.0 mask 255.255.255.252

```

#### Router2 (AS50)
```
router bgp 50
bgp router-id 3.3.3.3
neighbor 20.20.20.1 remote-as 35
network 192.168.20.0 mask 255.255.255.0

```

### Verify BGP Peering and Routing Table
Use the following commands on each router 

```
show ip bgp summary
```
This verifies if BGP peering is established between routers. Output for Router 1 is shown below 
<img width="551" height="222" alt="show ip bgp neighbor(router_!)" src="https://github.com/user-attachments/assets/cb0282fc-79d8-4ed3-97dc-24bf99198110" />

```
show ip route bgp

```
This would display routes learned via BGP. Output for router 1 is shown below 
<img width="551" height="56" alt="show ip route bgp(router_1)" src="https://github.com/user-attachments/assets/f359918e-bdbd-4686-9aa7-446e48ec4580" />

```
show ip bgp

```

This would list all BGP-advertised and learned prefixes. Output for router 1 is shown below 
<img width="547" height="184" alt="show ip bgp(router_1)" src="https://github.com/user-attachments/assets/9d682fd2-8ce4-4239-af2f-180c4f9b48fd" />

### Test Network Connectivity
Ping from hosts in AS20 to hosts in AS50 to confirm end-to-end reachability across autonomous systems. Results upon pinging from router 1 in AS20 to router 2 in AS50
<img width="549" height="87" alt="ping router 2 from router 1" src="https://github.com/user-attachments/assets/bf329038-2385-46e1-96e8-f79786676ab7" />

✅ Successful ping confirms that BGP has correctly exchanged routes between AS20 and AS50.

### IP Address Summary Table
## IP Address Summary Table

| Router | Interface | IP Address | Connected Network / AS | Role |
|--------|------------|-------------|--------------------------|------|
| **Router1 (AS20)** | g0/0 | 10.10.10.2/30 | AS35 | eBGP Peering with Router0 |
|  | g0/1 | 192.168.10.1/24 | AS20 | Local LAN |
| **Router0 (AS35)** | g0/0 | 10.10.10.1/30 | AS20 | eBGP Peering with Router1 |
|  | g0/1 | 20.20.20.1/30 | AS50 | eBGP Peering with Router2 |
| **Router2 (AS50)** | g0/0 | 20.20.20.2/30 | AS35 | eBGP Peering with Router0 |
|  | g0/1 | 192.168.20.1/24 | AS50 | Local LAN |

# Advanced Networking Notes

A practical reference for advanced networking concepts, written for revision, viva preparation, and cybersecurity fundamentals.

---

## Table of Contents

1. [VLANs](#1-vlans)
2. [NAT and PAT](#nat-and-pat)
2. [Trunking](#2-trunking)
3. [802.1Q VLAN Tagging](#3-8021q-vlan-tagging)
4. [Inter-VLAN Routing](#4-inter-vlan-routing)
5. [Spanning Tree Protocol (STP)](#5-spanning-tree-protocol-stp)
6. [EtherChannel](#6-etherchannel)
7. [Access Control Lists (ACLs)](#7-access-control-lists-acls)
8. [Port Forwarding](#8-port-forwarding)
9. [Forward Proxy vs Reverse Proxy](#9-forward-proxy-vs-reverse-proxy)
10. [Load Balancing](#10-load-balancing)
11. [Quality of Service (QoS)](#11-quality-of-service-qos)
12. [MTU](#12-mtu)
13. [Broadcast Domains vs Collision Domains](#13-broadcast-domains-vs-collision-domains)
14. [Firewalls](#14-firewalls)
15. [Default Gateway](#15-default-gateway)
16. [Routing](#16-routing)
17. [Switching](#17-switching)
18. [ICMP](#18-icmp)
19. [Quick Comparison Tables](#19-quick-comparison-tables)
20. [Viva Questions](#20-viva-questions)

---

# 1. VLANs

## What is a VLAN?

**VLAN = Virtual Local Area Network.**

A VLAN is a logical method of dividing a physical Layer 2 network into separate logical networks.

A single physical switch can contain multiple VLANs.

Example:

```text
                 Switch
          â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”
          â”‚        â”‚        â”‚
       VLAN 10   VLAN 20   VLAN 30
       Students    Staff    Servers
```

Although all devices may be connected to the same physical switch, VLANs logically separate them.

## Why are VLANs used?

VLANs are used for:

- Network segmentation
- Security
- Separating departments or groups
- Reducing unnecessary broadcast traffic
- Better network organization
- Creating separate logical networks without requiring separate physical switches

## VLANs and broadcast domains

Each VLAN represents a separate **broadcast domain**.

For example:

```text
VLAN 10 â†’ Broadcast Domain 1
VLAN 20 â†’ Broadcast Domain 2
VLAN 30 â†’ Broadcast Domain 3
```

A broadcast sent inside VLAN 10 normally does not cross into VLAN 20.

## Important point

Devices in different VLANs cannot communicate directly at Layer 2.

Communication between VLANs requires **Layer 3 routing**.

---

# 2. Trunking

## What is trunking?

A **trunk link** is a network link that carries traffic belonging to multiple VLANs.

Without trunking, separate physical links could be required for different VLANs.

With trunking:

```text
Switch A                         Switch B
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”                     â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚ VLAN 10  â”‚                     â”‚ VLAN 10  â”‚
â”‚ VLAN 20  â”‚==== Trunk Link ====â”‚ VLAN 20  â”‚
â”‚ VLAN 30  â”‚                     â”‚ VLAN 30  â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜                     â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

A single physical link can carry traffic from VLAN 10, VLAN 20, VLAN 30, and so on.

## Access port vs trunk port

### Access port

An access port normally carries traffic for **one VLAN**.

Example:

```text
PC â”€â”€â”€â”€â”€ Access Port â”€â”€â”€â”€â”€ Switch
             VLAN 10
```

### Trunk port

A trunk port carries traffic for **multiple VLANs**.

Example:

```text
Switch A â”€â”€â”€â”€â”€ Trunk â”€â”€â”€â”€â”€ Switch B
             VLAN 10
             VLAN 20
             VLAN 30
```

## Viva answer

> Trunking allows multiple VLANs to travel over a single physical network link.

---

# 3. 802.1Q VLAN Tagging

## What is 802.1Q?

**IEEE 802.1Q** is the standard used to identify VLAN membership by adding a VLAN tag to Ethernet frames.

This is especially important on trunk links.

Conceptually:

```text
Ethernet Frame
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚ Header  â”‚ 802.1Q   â”‚ Payload  â”‚  FCS    â”‚
â”‚         â”‚ VLAN Tag â”‚          â”‚         â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

The VLAN tag allows a receiving network device to determine which VLAN the frame belongs to.

## Why is tagging needed?

Suppose a trunk carries:

```text
VLAN 10
VLAN 20
VLAN 30
```

The receiving switch needs a way to identify which VLAN each frame belongs to.

802.1Q provides that identification.

## Key distinction

- **VLAN** = logical network segmentation
- **Trunking** = carrying multiple VLANs over one link
- **802.1Q** = VLAN tagging standard used to identify VLAN traffic

---

# 4. Inter-VLAN Routing

## What is Inter-VLAN Routing?

Different VLANs are different Layer 2 networks.

If devices in different VLANs need to communicate, a **Layer 3 device** must route the traffic.

That process is called **Inter-VLAN Routing**.

Example:

```text
VLAN 10                         VLAN 20
PC A                            PC B
192.168.10.10                   192.168.20.10
     â”‚                                â”‚
     â””â”€â”€â”€â”€â”€â”€ Switch / Layer 3 â”€â”€â”€â”€â”€â”€â”€â”˜
                    â”‚
                 Router
```

The router or Layer 3 switch provides a gateway for each VLAN.

## Router-on-a-stick

One traditional method is called **Router-on-a-Stick**.

A single physical router interface can use multiple logical subinterfaces, with each subinterface associated with a VLAN.

Conceptually:

```text
VLAN 10 â”€â”
         â”‚
VLAN 20 â”€â”¼â”€â”€ Trunk â”€â”€ Router
         â”‚
VLAN 30 â”€â”˜
```

## Layer 3 switch

A Layer 3 switch can also perform routing between VLANs.

This is commonly more efficient in larger networks.

## Viva answer

> Inter-VLAN routing allows devices in different VLANs to communicate through a router or Layer 3 switch.

---

# 5. Spanning Tree Protocol (STP)

## What is STP?

**STP = Spanning Tree Protocol.**

STP is used to prevent **Layer 2 switching loops**.

## Why are Layer 2 loops dangerous?

Suppose switches have redundant connections:

```text
        Switch A
        /      \
       /        \
  Switch B â”€â”€â”€ Switch C
```

There are multiple paths between switches.

Ethernet frames do not have a Layer 2 TTL equivalent that automatically stops them from circulating forever.

A loop can cause:

- Broadcast storms
- Duplicate frames
- MAC table instability
- Severe network performance problems

## What does STP do?

STP identifies redundant paths and logically blocks some of them.

Conceptually:

```text
        Switch A
        /      \
       /        \
  Switch B â”€Xâ”€ Switch C
```

`X` represents a logically blocked redundant path.

If an active path fails, STP can converge and allow a previously blocked path to be used.

## Key idea

STP provides:

- Loop prevention
- Redundancy
- Automatic recovery from certain link failures

## Viva answer

> STP prevents Layer 2 loops by creating a loop-free logical topology while maintaining redundant paths for fault tolerance.

---

# 6. EtherChannel

## What is EtherChannel?

EtherChannel combines multiple physical Ethernet links into one logical link.

Example:

```text
Switch A                         Switch B
   â•‘                                â•‘
   â•‘â•â•â•â•â•â•â•â• Link 1 â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•‘
   â•‘â•â•â•â•â•â•â•â• Link 2 â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•‘
   â•‘â•â•â•â•â•â•â•â• Link 3 â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•‘
```

The group is treated logically as one connection.

## Benefits

### Increased bandwidth

Multiple links can provide greater aggregate capacity.

### Redundancy

If one physical link fails, other links in the group can continue carrying traffic.

### Simplified logical topology

The group is treated as one logical link by the network.

## Common protocols

### LACP

**LACP = Link Aggregation Control Protocol**

An industry-standard protocol for negotiating link aggregation.

### PAgP

**PAgP = Port Aggregation Protocol**

A Cisco proprietary protocol for EtherChannel negotiation.

## Viva answer

> EtherChannel combines multiple physical Ethernet links into one logical link to provide increased bandwidth and redundancy.

---

# 7. Access Control Lists (ACLs)

## What is an ACL?

**ACL = Access Control List.**

An ACL is a set of rules used to **permit or deny network traffic** based on defined criteria.

Rules can examine things such as:

- Source IP address
- Destination IP address
- Protocol
- Source port
- Destination port

Example:

```text
ALLOW 192.168.1.0/24 â†’ Server
DENY  10.0.0.0/8     â†’ Server
```

## ACL processing

A device evaluates traffic against ACL rules.

Depending on the configuration, traffic can be:

- Permitted
- Denied
- Filtered according to specific conditions

## ACL vs Firewall

ACLs are primarily traffic-filtering rules associated with network interfaces or devices.

A firewall is a broader security control that can provide stateful inspection, application-aware filtering, logging, and other security capabilities depending on the product.

## Viva answer

> An ACL is a set of rules used to permit or deny network traffic according to criteria such as IP addresses, protocols, and ports.

---

# NAT and PAT

## NAT

**NAT = Network Address Translation.**

NAT translates IP addresses between private and public networks. It is commonly performed by a router or firewall at the boundary between a private network and the Internet.

Example:

```text
Private Network                    Internet

192.168.1.10 â”€â”
192.168.1.11 â”€â”¼â”€â”€ Router/NAT â”€â”€â”€â†’ Public IP
192.168.1.12 â”€â”˜
```

A private address such as `192.168.1.10` is not directly routable across the public Internet. NAT allows internal devices to communicate with external networks by translating their addresses.

## Types of NAT

### Static NAT

Static NAT creates a fixed **one-to-one mapping** between a private IP and a public IP.

```text
192.168.1.10 â†” 203.0.113.10
```

Useful when an internal device needs a consistent public mapping, such as a publicly reachable server.

### Dynamic NAT

Dynamic NAT maps private IP addresses to public addresses from a configured pool.

```text
192.168.1.10 â†’ 203.0.113.10
192.168.1.11 â†’ 203.0.113.11
```

The mapping is assigned dynamically and depends on the available public address pool.

## PAT

**PAT = Port Address Translation.**

PAT is commonly called **NAT Overload**.

PAT allows many private devices to share a single public IPv4 address by using different port numbers to distinguish their connections.

Example:

```text
192.168.1.10:5000 â”€â”
192.168.1.11:5001 â”€â”¼â”€â”€â†’ 203.0.113.10
192.168.1.12:5002 â”€â”˜
```

The NAT device can translate them into different public source ports:

```text
192.168.1.10:5000 â†’ 203.0.113.10:40001
192.168.1.11:5001 â†’ 203.0.113.10:40002
192.168.1.12:5002 â†’ 203.0.113.10:40003
```

The device keeps a translation table so that returning traffic can be mapped back to the correct internal host.

## NAT vs PAT

| NAT | PAT |
|---|---|
| Translates IP addresses | Translates IP addresses and uses port numbers |
| Can provide one-to-one or pool-based mappings | Many private hosts can share one public IP |
| Static NAT and Dynamic NAT are examples | Also called NAT Overload |
| Public IPs may be required per mapping | Conserves public IPv4 addresses much more efficiently |

## NAT vs Firewall

NAT and firewalls are different technologies.

**NAT** primarily performs address/port translation.

**A firewall** filters traffic according to security policies.

NAT may hide internal addressing, but **NAT itself is not a firewall**.

## Port Forwarding and NAT

Port forwarding is a form of destination address/port translation that allows incoming traffic from a public address and port to reach a specific private host and service.

Example:

```text
203.0.113.10:8080
        â†“
Router/NAT
        â†“
192.168.1.50:80
```

## Viva answers

**Q: What is NAT?**

> NAT stands for Network Address Translation. It translates IP addresses between private and public networks, allowing private hosts to communicate with external networks.

**Q: What is PAT?**

> PAT stands for Port Address Translation. It allows multiple private devices to share a single public IP address by using different port numbers to distinguish their connections.

**Q: What is NAT Overload?**

> NAT Overload is another name for PAT.

**Q: Which technology is commonly used in home networks?**

> PAT/NAT Overload, because many devices can share one public IPv4 address.

**Q: Is NAT a firewall?**

> No. NAT performs address and port translation, while a firewall controls traffic according to security rules.

---

# 8. Port Forwarding

## What is Port Forwarding?

Port forwarding allows traffic arriving at a router's public IP and specific port to be forwarded to a particular internal device and port.

Example:

```text
Internet
    â”‚
Public IP: 203.0.113.10:8080
    â”‚
    â–¼
 Router
    â”‚
    â–¼
192.168.1.50:80
Web Server
```

The router can translate:

```text
203.0.113.10:8080
        â†“
192.168.1.50:80
```

## Why is it used?

Common uses include making an internal service reachable from outside the private network.

Examples:

- Web servers
- Game servers
- Remote administration services
- Self-hosted applications

## Security consideration

Opening a port to the Internet exposes a service to external traffic.

Therefore, port forwarding should be used carefully and combined with appropriate authentication, patching, firewall rules, and access controls.

## Viva answer

> Port forwarding maps a port on a public IP address to a specific IP address and port on a private network.

---

# 9. Forward Proxy vs Reverse Proxy

## Forward Proxy

A forward proxy sits between **clients and external servers**.

```text
Client â†’ Forward Proxy â†’ Internet â†’ Server
```

The proxy represents the client.

Possible functions:

- Web filtering
- Access control
- Caching
- Logging
- Policy enforcement
- Hiding the client's direct network identity from the destination in some configurations

## Reverse Proxy

A reverse proxy sits between **clients and backend servers**.

```text
Client â†’ Reverse Proxy â†’ Backend Server
                       â†’ Backend Server
                       â†’ Backend Server
```

The reverse proxy represents the server side.

Possible functions:

- Load balancing
- TLS termination
- Caching
- Routing requests
- Security controls
- Hiding backend infrastructure

## Easy way to remember

> Forward proxy = represents the client.

> Reverse proxy = represents the server.

---

# 10. Load Balancing

## What is Load Balancing?

Load balancing distributes incoming traffic across multiple servers or resources.

Without load balancing:

```text
Users
  â”‚
  â–¼
Server
```

With load balancing:

```text
             â”Œâ”€â”€ Server 1
Users â”€â”€ Loadâ”œâ”€â”€ Server 2
             â””â”€â”€ Server 3
```

## Why use load balancing?

### Performance

Traffic is distributed instead of overwhelming one server.

### Availability

If one server fails, traffic can potentially be sent to healthy servers.

### Scalability

More servers can be added as demand increases.

## Common strategies

Basic load-balancing algorithms include:

- Round Robin
- Weighted Round Robin
- Least Connections
- IP Hash

## Viva answer

> Load balancing distributes incoming traffic across multiple servers to improve performance, availability, and scalability.

---

# 11. Quality of Service (QoS)

## What is QoS?

**QoS = Quality of Service.**

QoS manages and prioritizes network traffic so important or delay-sensitive traffic receives appropriate treatment during congestion.

Example:

```text
VoIP       â†’ High priority
Video      â†’ High/medium priority
Web        â†’ Normal priority
Downloads  â†’ Lower priority
```

## Why is QoS important?

Some applications are especially sensitive to:

- Delay
- Jitter
- Packet loss

Voice and real-time video are common examples.

A large file download can usually tolerate some delay. A voice call cannot tolerate unpredictable delay nearly as well.

## Viva answer

> QoS prioritizes and manages network traffic to improve performance for applications sensitive to delay, jitter, and packet loss.

---

# 12. MTU

## What is MTU?

**MTU = Maximum Transmission Unit.**

MTU is the maximum size of a packet that can normally be transmitted over a particular network link without requiring fragmentation at that point.

For standard Ethernet, a common MTU is:

**1500 bytes**

## Why does MTU matter?

If a packet is larger than the supported MTU, it may need to be fragmented or otherwise handled according to the protocol and network conditions.

Incorrect MTU settings can cause:

- Connectivity problems
- Performance issues
- Problems with certain applications
- Packet fragmentation or drops

## Path MTU

The **Path MTU** is the largest packet size that can travel across an entire network path without fragmentation.

## Viva answer

> MTU is the maximum packet size that can be transmitted over a network link without fragmentation at that link.

---

# 13. Broadcast Domains vs Collision Domains

## Broadcast Domain

A broadcast domain is the set of devices that can receive a Layer 2 broadcast.

Example:

```text
PC A â”€â”
PC B â”€â”¼â”€â”€ Switch
PC C â”€â”˜
```

A broadcast sent within that network can reach the devices in the same broadcast domain.

### What separates broadcast domains?

- Routers
- VLANs

Each VLAN is normally its own broadcast domain.

---

## Collision Domain

A collision domain is a network segment where Ethernet frames could potentially collide.

### Hubs

With traditional Ethernet hubs, devices shared the same collision domain.

### Modern switches

Modern full-duplex switched Ethernet largely eliminates collisions on individual point-to-point switch links.

## Easy distinction

> Broadcast domain = who receives a broadcast?

> Collision domain = where can Ethernet frames collide?

---

# 14. Firewalls

## What is a Firewall?

A firewall is a security mechanism that monitors and controls network traffic according to predefined security rules.

It can make decisions using information such as:

- Source IP
- Destination IP
- Source port
- Destination port
- Protocol
- Connection state
- Application-level information, depending on the firewall

A firewall can allow, deny, or inspect traffic.

## Stateless Firewall

A stateless firewall evaluates packets independently using configured rules.

It does not maintain the state of network connections.

## Stateful Firewall

A stateful firewall keeps track of active connections.

For example:

```text
Client â”€â”€ SYN â”€â”€â†’ Server
        Firewall
           â”‚
      Tracks state
```

When the server responds, the firewall can determine whether the traffic belongs to an established connection.

## Firewall vs NAT

They are not the same thing.

**NAT:**
Translates addresses and/or ports.

**Firewall:**
Controls traffic according to security rules.

NAT can hide internal addressing, but **NAT itself is not a firewall**.

## Viva answer

> A firewall monitors and filters network traffic according to security policies to control which connections are allowed or blocked.

---

# 15. Default Gateway

## What is a Default Gateway?

A default gateway is the router or Layer 3 device that a host uses to reach destinations outside its local network when no more specific route is available.

Example:

```text
PC
192.168.1.10
     â”‚
     â–¼
Default Gateway
192.168.1.1
     â”‚
     â–¼
Other Networks / Internet
```

If the destination is on the same subnet, the host can communicate directly.

If the destination is outside the local subnet, the host sends the packet toward its default gateway.

## Example

PC:

```text
IP:       192.168.1.10
Mask:     255.255.255.0
Gateway:  192.168.1.1
```

Destination:

```text
192.168.1.20
```

Same subnet, so the PC can communicate directly.

Destination:

```text
8.8.8.8
```

Different network, so the packet is sent to the default gateway.

## Viva answer

> The default gateway is the Layer 3 device a host uses to forward traffic destined for networks outside its local network when no more specific route exists.

---

# 16. Routing

## What is Routing?

Routing is the process of determining the path that packets should take from a source network to a destination network.

Routers make forwarding decisions primarily using the **destination IP address**.

Example:

```text
PC
192.168.1.10
    â”‚
    â–¼
Router A
    â”‚
    â–¼
Router B
    â”‚
    â–¼
Server
192.168.10.10
```

## Routing Table

Routers maintain a routing table containing information about reachable networks.

Example:

```text
Destination       Next Hop
192.168.2.0/24    192.168.1.1
192.168.3.0/24    192.168.1.2
0.0.0.0/0         ISP Router
```

## Next Hop

The **next hop** is the next Layer 3 device to which the router forwards a packet on its journey toward the destination.

## Default Route

A default route is used when there is no more specific route available.

IPv4 default route:

```text
0.0.0.0/0
```

IPv6 default route:

```text
::/0
```

## Static Routing

Routes are manually configured by an administrator.

Advantages:

- Simple for small networks
- Predictable
- No routing protocol overhead

Disadvantages:

- Difficult to maintain in large networks
- Changes must be configured manually

## Dynamic Routing

Routers learn routes automatically using routing protocols.

Examples:

- RIP
- OSPF
- BGP

---

# 17. Switching

## What is Switching?

Switching is the process of forwarding Ethernet frames within a Layer 2 network.

A Layer 2 switch primarily uses **MAC addresses** to make forwarding decisions.

## MAC Address Table

A switch maintains a MAC address table.

Example:

```text
MAC Address         Port
AA:AA:AA:AA:AA:AA   Fa0/1
BB:BB:BB:BB:BB:BB   Fa0/2
CC:CC:CC:CC:CC:CC   Fa0/3
```

The switch learns which MAC address is reachable through which port.

## Switch vs Router

### Switch

- Primarily Layer 2
- Uses MAC addresses
- Forwards Ethernet frames
- Connects devices within LANs

### Router

- Layer 3
- Uses IP addresses
- Routes packets between networks
- Separates broadcast domains

---

# 18. ICMP

## What is ICMP?

**ICMP = Internet Control Message Protocol.**

ICMP is used for network error reporting, diagnostics, and control messages.

## Ping

The `ping` utility commonly uses:

- ICMP Echo Request
- ICMP Echo Reply

Example:

```text
Host A â”€â”€ ICMP Echo Request â”€â”€â†’ Host B
Host A â†â”€â”€ ICMP Echo Reply â”€â”€â”€ Host B
```

This can help determine whether a host is reachable and measure round-trip time.

## Common ICMP messages

- Echo Request
- Echo Reply
- Destination Unreachable
- Time Exceeded

## Traceroute

Traceroute/tracert uses mechanisms involving TTL/hop-limit expiration and ICMP responses, although the exact implementation varies by operating system.

## Viva answer

> ICMP is used for network diagnostics, error reporting, and control messages. Ping commonly uses ICMP Echo Request and Echo Reply.

---

# 19. Quick Comparison Tables

## VLAN vs Subnet

| VLAN | Subnet |
|---|---|
| Layer 2 concept | Layer 3 concept |
| Creates a logical broadcast domain | Defines an IP network |
| Uses VLAN IDs | Uses IP address and subnet mask/prefix |
| Separates Ethernet traffic | Separates IP networks |

They are commonly used together.

Example:

```text
VLAN 10 â†’ 192.168.10.0/24
VLAN 20 â†’ 192.168.20.0/24
```

---

## Switch vs Router

| Switch | Router |
|---|---|
| Primarily Layer 2 | Layer 3 |
| Uses MAC addresses | Uses IP addresses |
| Forwards frames | Routes packets |
| Connects devices in a LAN | Connects different networks |
| VLANs can separate its broadcast domains | Separates broadcast domains |

---

## Forward Proxy vs Reverse Proxy

| Forward Proxy | Reverse Proxy |
|---|---|
| Represents clients | Represents servers |
| Client â†’ Proxy â†’ Internet | Client â†’ Proxy â†’ Server |
| Client-side use | Server-side use |
| Filtering/caching/access control | Load balancing/TLS/caching/security |

---

## NAT vs Firewall

| NAT | Firewall |
|---|---|
| Translates addresses/ports | Filters traffic |
| Solves addressing/connectivity problems | Enforces security policy |
| Can provide address hiding | Provides traffic control |
| Not inherently a firewall | Security mechanism |

---

## Access Port vs Trunk Port

| Access Port | Trunk Port |
|---|---|
| Normally carries one VLAN | Carries multiple VLANs |
| Commonly connects end devices | Commonly connects switches/routers |
| Frames are associated with one VLAN | VLAN identification is carried across the trunk |

---

# 20. Viva Questions

## VLAN

**Q: What is a VLAN?**

A VLAN is a logical Layer 2 network that separates devices into different broadcast domains.

**Q: Why are VLANs used?**

For segmentation, security, organization, and broadcast-domain separation.

**Q: Can devices in different VLANs communicate directly?**

Not at Layer 2. They require Layer 3 routing.

---

## Trunking

**Q: What is a trunk?**

A trunk is a link that carries traffic from multiple VLANs.

**Q: What standard is commonly used for VLAN tagging?**

IEEE 802.1Q.

---

## STP

**Q: Why is STP needed?**

To prevent Layer 2 switching loops.

**Q: What can a Layer 2 loop cause?**

Broadcast storms, duplicate frames, and MAC table instability.

---

## EtherChannel

**Q: What is EtherChannel?**

It combines multiple physical Ethernet links into one logical link.

**Q: Name a protocol used with EtherChannel.**

LACP.

---

## ACL

**Q: What is an ACL?**

A list of rules used to permit or deny network traffic based on defined criteria.

---

## Port Forwarding

**Q: What is port forwarding?**

It maps traffic arriving at a public IP and port to a specific private IP and port.

---

## Proxy

**Q: What is the difference between a forward proxy and a reverse proxy?**

A forward proxy represents the client, while a reverse proxy represents the server.

---

## Load Balancing

**Q: Why use a load balancer?**

To distribute traffic across multiple servers, improving performance, availability, and scalability.

---

## QoS

**Q: What is QoS?**

QoS manages and prioritizes network traffic, especially for applications sensitive to delay, jitter, or packet loss.

---

## MTU

**Q: What does MTU stand for?**

Maximum Transmission Unit.

**Q: What is a common Ethernet MTU?**

1500 bytes.

---

## Broadcast Domain

**Q: What is a broadcast domain?**

The group of devices that can receive a Layer 2 broadcast.

**Q: What separates broadcast domains?**

Routers and VLANs.

---

## Default Gateway

**Q: What is a default gateway?**

The Layer 3 device a host uses to reach destinations outside its local network when no more specific route exists.

---

# Final Revision Map

A useful way to connect the concepts:

```text
                    NETWORK
                       â”‚
          â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
          â”‚                         â”‚
       Layer 2                   Layer 3
          â”‚                         â”‚
   â”Œâ”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”          â”Œâ”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”
   â”‚             â”‚          â”‚               â”‚
 Switch        VLAN       IP Address      Routing
   â”‚             â”‚          â”‚               â”‚
 MAC          Broadcast   Subnet         Router
 Address       Domain     Mask            Table
   â”‚
   â”œâ”€â”€ Trunking
   â”œâ”€â”€ 802.1Q
   â”œâ”€â”€ STP
   â””â”€â”€ EtherChannel

          Security / Services
                 â”‚
      â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
      â”‚          â”‚           â”‚
   Firewall     ACL        NAT
      â”‚                      â”‚
      â”‚                Port Forwarding
      â”‚
   Filtering

       Application / Traffic Management
                 â”‚
      â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
      â”‚          â”‚          â”‚
   Proxy     Load Balancer  QoS
```

## One-line memory triggers

- **VLAN** â†’ Logical Layer 2 segmentation
- **Trunk** â†’ Multiple VLANs on one link
- **802.1Q** â†’ VLAN tagging
- **Inter-VLAN routing** â†’ Communication between VLANs
- **STP** â†’ Prevent Layer 2 loops
- **EtherChannel** â†’ Multiple links become one logical link
- **ACL** â†’ Permit/deny traffic using rules
- **Port forwarding** â†’ Public port to private service
- **Forward proxy** â†’ Represents client
- **Reverse proxy** â†’ Represents server
- **Load balancing** â†’ Distributes traffic
- **QoS** â†’ Prioritizes traffic
- **MTU** â†’ Maximum transmission size
- **Broadcast domain** â†’ Who receives broadcasts
- **Collision domain** â†’ Where collisions can occur
- **Firewall** â†’ Filters traffic based on security policy
- **Default gateway** â†’ Exit point to other networks
- **Routing** â†’ Determines where packets go
- **Switching** â†’ Forwards frames using MAC addresses
- **ICMP** â†’ Diagnostics and network control/error messages

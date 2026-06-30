# 🌐 Week 05 – Subnetting, NAT, PAT, Network Monitoring & Troubleshooting

This continues naturally from your previous networking notes.

## Why These Topics Matter

Network administrators and cybersecurity professionals use subnetting, NAT, monitoring tools, and troubleshooting techniques to:

* Improve network performance
* Conserve IP addresses
* Increase security
* Detect attacks
* Diagnose network problems

## Subnetting

### What is a Subnet?

A subnet (subnetwork) is a smaller network created from a larger network.

Example:
```text
192.168.0.0/24
```
can be divided into:
```text
192.168.0.0/28
```
which becomes a subnet of the original network.

## Why Use Subnetting?

### Efficient IP Usage

Avoid wasting IP addresses.

### Better Security

Separate departments and systems.

### Better Performance

Reduces broadcast traffic.

## Network vs Host Portion

An IPv4 address contains:
```mermaid
flowchart LR
A[Network Portion] --> B[Host Portion]
Network Portion → Identifies the network
Host Portion → Identifies the device
```
## Subnet Mask

A subnet mask separates the network portion from the host portion.

Example:
```text
255.255.255.0
```
Binary:
```text
11111111.11111111.11111111.00000000
```
* 1s = Network bits
* 0s = Host bits

## Common CIDR Reference

|CIDR|	Subnet Mask|	Usable Hosts|
|---|---|---|
|/24|	255.255.255.0|	254|
|/25|	255.255.255.128|	126|
|/26|	255.255.255.192|	62|
|/27|	255.255.255.224|	30|
|/28|	255.255.255.240|	14|
|/29|	255.255.255.248|	6|
|/30|	255.255.255.252|	2|

### Subnetting Formula

Number of Subnets:

Where:

* n = borrowed host bits

### Host Formula

Usable Hosts:

Where:

* h = remaining host bits
* subtract 2 for:
    * Network Address
    * Broadcast Address

Example

Original Network:

192.168.1.0/24

Need:

4 Subnets

Borrow:

2 Host Bits

New Network:

192.168.1.0/26

Creates:

Subnet	Range
192.168.1.0/26	1 - 62
192.168.1.64/26	65 - 126
192.168.1.128/26	129 - 190
192.168.1.192/26	193 - 254

Network Address Translation (NAT)
What is NAT?

NAT (Network Address Translation) translates private IP addresses into public IP addresses before traffic is sent to the Internet.

Why NAT Exists

Home devices use private IPs:

192.168.1.x

The Internet only sees:

One Public IP
NAT Process
flowchart LR
A[Private Devices]
--> B[Router NAT]
--> C[Public Internet]
Types of NAT
Static NAT

One private IP always maps to the same public IP.

Dynamic NAT

Uses a pool of public IP addresses.

PAT (Port Address Translation)

Many private IPs share one public IP using different port numbers.

PAT (Port Address Translation)

PAT maps multiple private IP addresses to a single public IP address.

Uses:

Source IP
Source Port

to uniquely identify connections.

PAT Example
flowchart LR

A[Laptop]
B[Phone]
C[Tablet]

A --> D[Router PAT]
B --> D
C --> D

D --> E[Single Public IP]
Network Monitoring
What is Network Monitoring?

The continuous observation of network performance, availability, and security.

Goals:

Detect failures
Detect attacks
Monitor traffic
Improve performance

Wireshark
What is Wireshark?

Wireshark is a packet analysis tool used to capture and inspect network traffic.

Features
Packet Capture
Deep Protocol Inspection
Filtering
Statistics
Stream Reassembly
Uses
Troubleshooting
Security Analysis
Incident Investigation

Ettercap
What is Ettercap?

Ettercap is a network analysis and security testing tool.

Features
Packet Sniffing
ARP Poisoning
Man-in-the-Middle Testing

⚠️ Commonly used in cybersecurity labs.

Common Network Problems
Problem	Possible Cause
Connectivity Issues	Bad cable, configuration errors
Slow Network	Congestion, interference
IP Conflict	Duplicate IP addresses
DNS Failure	Incorrect DNS settings
NIC Problems	Driver or hardware failure
Wireless Problems	Weak signal
Firewall Issues	Blocked traffic
VPN Issues	Incorrect VPN configuration

Troubleshooting Tools
Ping

Purpose:

Check if a host is reachable

Example:

ping google.com
Traceroute / Tracert

Purpose:

Show packet path to destination

Example:

tracert google.com

Windows

traceroute google.com

Linux

Netstat

Purpose:

Display network connections and ports

Example:

netstat -an
Nslookup

Purpose:

Query DNS records

Example:

nslookup google.com
Dig

Linux DNS troubleshooting tool.

Example:

dig google.com

Cybersecurity Connections
Topic	Security Relevance
Subnetting	Network segmentation
NAT	Hides internal IP structure
PAT	Conserves public IPs
Wireshark	Packet analysis
Ettercap	MITM testing
Ping	Connectivity verification
Traceroute	Route investigation
Nslookup	DNS troubleshooting
Key Terms to Remember
Subnet = Smaller network inside a larger network

CIDR = /24, /25, /26 etc.

NAT = Network Address Translation

PAT = Port Address Translation

Wireshark = Packet Analyzer

Ettercap = Network Analysis Tool

Ping = Reachability Test

Traceroute = Path Discovery

Netstat = Connection Monitoring

Nslookup = DNS Query Tool

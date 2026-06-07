# Table of Contents
* What is an IP Address?
* What is a Subnet?
* What is a Subnet Mask?
* How Subnetting Works
* CIDR Notation
* Calculating Hosts and Networks
* Subnetting Examples
* VLSM (Variable Length Subnet Masking)
* Subnetting vs VLSM
* Quick Reference Tables
* Interview and Exam Questions

## 1. What is an IP Address?

An IP address is a logical address assigned to a device on a network.

Example:

```text
192.168.1.10
```

Every IP address contains:

- Network Portion

- Host Portion

Example:
```text
192.168.1.10/24
```

Network:
```text
192.168.1
```

Host:
```text
10
```

## 2. What is a Subnet?

A subnet is a smaller network created from a larger network.

Instead of having one large network:
```text
192.168.1.0/24
```

You can divide it into:
```text
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

Benefits:

* Better organization
* Better security
* Reduced broadcast traffic
* More efficient IP usage
* 
## 3. What is a Subnet Mask?

A subnet mask identifies which part of an IP address is:
```text
Network
Host
```
Example:
```text
IP Address:
192.168.1.10

Subnet Mask:
255.255.255.0
```

Binary representation:
```text
IP:
11000000.10101000.00000001.00001010

Mask:
11111111.11111111.11111111.00000000
```

Rule:
```text
1 = Network Bit
0 = Host Bit
```

Since the first 24 bits are 1:
```text
/24
```

## 4. How Subnetting Works

Suppose we have:
```text
192.168.1.0/24
```

This contains:
```text
256 total addresses
254 usable hosts
```

We need 4 subnets.

Borrow 2 host bits:
```text
/24 + 2 = /26
```

New subnet mask:
```text
255.255.255.192
```

### Finding Block Size

Formula:
```text
256 - Last Octet
```

Example:
```text
255.255.255.192

256 - 192 = 64
```

Block size:
```text
64
```

Subnet increments:
```text
0
64
128
192
```

Subnets:
```text
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

## 5. CIDR (Classless Inter-Domain Routing)

CIDR replaced old Class A, B, and C addressing.

Instead of:
```text
Class A
Class B
Class C
```

We use:
```text
/8
/16
/24
/27
/30
```

Example:
```text
192.168.1.0/24
```

The "/24" means:
```text
24 bits = Network
8 bits = Host
```

## CIDR Examples

|CIDR	| Subnet Mask|
|---|---|
|/24	| 255.255.255.0|
|/25	| 255.255.255.128|
|/26	| 255.255.255.192|
|/27	| 255.255.255.224|
|/28	| 255.255.255.240|
|/29	| 255.255.255.248|
|/30	| 255.255.255.252|

## 6. Calculating Hosts

Formula:
```text
Hosts = 2^(Host Bits) - 2
```

Why subtract 2?
```text
1 = Network Address
1 = Broadcast Address
```

Example 1
```text
/24
```

Host bits:
```text
32 - 24 = 8
```

Hosts:
```text
2^8 - 2
= 256 - 2
= 254
```

Example 2
```text
/27
```

Host bits:
```text
32 - 27 = 5
```

Hosts:
```text
2^5 - 2
= 32 - 2
= 30
```

## 7. Complete Subnetting Example

Given:
```text
192.168.1.0/24
```

Need:
```text
4 subnets
```

Step 1:

Borrow 2 bits.
```text
/24 → /26
```

Step 2:

Find block size.
```text
256 - 192 = 64
```

Step 3:

Generate subnets.

|Network	| First Host	| Last Host	| Broadcast|
|---|---|---|---|
|192.168.1.0/26	| .1	| .62	| .63
|192.168.1.64/26	| .65	| .126	| .127
|192.168.1.128/26	| .129	| .190	| .191
|192.168.1.192/26	| .193	| .254	| .255

## 8. What is VLSM?

VLSM = Variable Length Subnet Masking

Allows different subnet sizes within the same network.

Traditional subnetting:
```text
/26
/26
/26
/26
```

VLSM:
```text
/26
/27
/28
/29
```

Much more efficient.

## 9. VLSM Example

Network:
```tex
192.168.1.0/24
```

Requirements:

|Department	| Hosts Needed|
|---|---|
|HR	| 50|
|Sales	| 20|
|IT	| 10|
|Finance	| 5|

Step 1: Sort Largest to Smallest
```text
50
20
10
5
```

Step 2: Allocate Networks
### HR

Needs 50 hosts.
```text
/26 = 62 hosts
```

Assign:
```text
192.168.1.0/26
```

Range:
```text
192.168.1.1 - 192.168.1.62
```

Broadcast:
```text
192.168.1.63
```

### Sales

Needs 20 hosts.
```text
/27 = 30 hosts
```

Assign:
```text
192.168.1.64/27
```

Range:
```text
192.168.1.65 - 192.168.1.94
```

Broadcast:
```text
192.168.1.95
```

### IT

Needs 10 hosts.
```text
/28 = 14 hosts
```

Assign:
```text
192.168.1.96/28
```

Range:
```text
192.168.1.97 - 192.168.1.110
```

Broadcast:
```text
192.168.1.111
```

### Finance

Needs 5 hosts.
```text
/29 = 6 hosts
```

Assign:
```text
192.168.1.112/29
```

Range:
```text
192.168.1.113 - 192.168.1.118
```

Broadcast:
```text
192.168.1.119
```

## 10. Subnetting vs VLSM

|Feature	| Subnetting	| VLSM|
|---|---|---|
|Subnet Size	| Same	| Different|
|Flexibility	| Low	| High|
|IP Utilization	| Less Efficient | More Efficient|
|Complexity	| Easy	| Moderate|
|Real-World Usage	| Rare	| Very Common|

## 11. Quick Reference Table

|CIDR|	Mask|	Hosts|
|---|---|---|
|/24	| 255.255.255.0	| 254|
|/25	| 255.255.255.128	| 126|
|/26	| 255.255.255.192	| 62|
|/27	| 255.255.255.224	| 30|
|/28	| 255.255.255.240	| 14|
|/29	| 255.255.255.248	| 6|
|/30	| 255.255.255.252	| 2|

## Common Interview Questions

### What is a subnet mask?

A subnet mask identifies the network and host portions of an IP address.

### What is CIDR?

CIDR (Classless Inter-Domain Routing) uses a prefix length such as /24 to define the network portion of an IP address.

### What is VLSM?

VLSM (Variable Length Subnet Masking) allows multiple subnet sizes within the same network, improving IP address utilization.

### Why subtract 2 when calculating hosts?

Because one address is reserved for the network address and one for the broadcast address.

### Can 192.168.1.10/24 communicate directly with 192.168.2.10/24?

No. They are in different networks and require a router to communicate.

## Cheat Sheet
```text
Hosts = 2^(32-CIDR) - 2

Block Size = 256 - Last Octet

Network Address = First Address

Broadcast Address = Last Address

First Host = Network + 1

Last Host = Broadcast - 1
```

# 🌐 Week 04 - OSI Model, TCP/IP, Network Protocols, DNS & DHCP
Why This Matters for Cybersecurity

Most cyber attacks target network protocols and services.

Examples:

* Phishing → Application Layer
* ARP Spoofing → Data Link Layer
* DDoS → Transport Layer
* DNS Poisoning → DNS Service

Understanding networking helps security professionals identify where attacks occur and how to defend against them.

The OSI Model

OSI stands for Open Systems Interconnection.

It is a theoretical framework that explains how devices communicate across a network using seven layers. Each layer performs a specific task.

OSI Stack
flowchart TD

L7["7 - Application"]
L6["6 - Presentation"]
L5["5 - Session"]
L4["4 - Transport"]
L3["3 - Network"]
L2["2 - Data Link"]
L1["1 - Physical"]

L7 --> L6 --> L5 --> L4 --> L3 --> L2 --> L1
Mnemonic
All People Seem To Need Data Processing

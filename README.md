# Enterprise Organization Network — Cisco Packet Tracer

## 📌 Project Overview

This project is a complete enterprise network design built from scratch using Cisco Packet Tracer.

The goal of the project is to combine the networking concepts learned from the previous modules into one realistic organization network.

The network is being designed step-by-step with a focus on:

- Enterprise network architecture
- Cisco switching
- VLAN segmentation
- STP/RSTP
- EtherChannel
- Inter-VLAN Routing
- OSPF
- DHCP
- SSH
- NTP
- SYSLOG
- TFTP
- Network management
- Troubleshooting
- Redundancy and high availability

> **Project status:** In Progress 🚧

---

# 🏗️ Current Network Architecture

The current physical topology consists of:

```text
                         R1
                          |
                         R2
                       /    \
                      /      \
             CORE-SW1        CORE-SW2
              / | \            / | \
             /  |  \          /  |  \
           SW1 SW2 SW3      SW4 SW5 SW6
             |
        Server Farm

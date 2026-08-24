Quyidagini to‘liq nusxalab GitHub'ga joylashtir:

# 🏦 Enterprise Bank Network — Cisco Packet Tracer

## 📌 Project Overview

Ushbu loyiha Cisco Packet Tracer muhitida real enterprise/bank tarmog‘iga yaqin qilib ishlab chiqilgan.

Loyihaning asosiy maqsadi:

- VLAN orqali foydalanuvchilarni segmentatsiya qilish
- Access va Trunk portlarni sozlash
- Native VLAN ishlatish
- Layer 3 Switching va SVI orqali routing tashkil qilish
- Inter-VLAN Routing
- OSPF Dynamic Routing
- Core va Access layer arxitekturasini yaratish
- End-to-End connectivity testlarini bajarish

---

# 🏗️ Network Architecture

Loyihaning asosiy arxitekturasi:

```text
                         ┌─────────────┐
                         │     R1      │
                         │   Router    │
                         └──────┬──────┘
                                │
                           OSPF /30
                                │
                         ┌──────┴──────┐
                         │     R2      │
                         │   Router    │
                         └──────┬──────┘
                                │
                           OSPF /30
                                │
                         ┌──────┴──────┐
                         │   CoreSW1   │
                         │ L3 Switch   │
                         └───┬────┬────┘
                             │    │
                    Trunk    │    │    Trunk
                             │    │
                         ┌───┘    └───┐
                         │             │
                    Access SWs     CoreSW2
                         │
                    End Devices
🔢 VLAN Design
VLAN	Name	Purpose	Gateway
10	MANAGEMENT	Management	10.10.10.1/24
20	FINANCE	Finance	10.10.20.1/24
30	HR	Human Resources	10.10.30.1/24
40	SALES	Sales	10.10.40.1/24
50	IT	IT Department	10.10.50.1/24
60	GUEST	Guest Network	10.10.60.1/24
70	SERVERS	Servers	10.10.70.1/24
99	NATIVE-MGMT	Native / Management	—
🔌 CoreSW1 Port Mapping
Gi0/1 → R2 Gi0/1
Gi0/2 → CoreSW2 Gi0/2

Fa0/1 → AccessSW1 Fa0/1
Fa0/2 → AccessSW2 Fa0/1
Fa0/3 → AccessSW3 Fa0/1

CoreSW1 uplinks:

Fa0/1 → Trunk → Native VLAN 99
Fa0/2 → Trunk → Native VLAN 99
Fa0/3 → Trunk → Native VLAN 99
🌐 Layer 3 Addressing
CoreSW1
Gi0/1 → 10.0.0.6/30
Gi0/2 → 10.0.0.13/30

VLAN 10 → 10.10.10.1/24
VLAN 20 → 10.10.20.1/24
R2
Gi0/0 → 10.0.0.2/30
Gi0/1 → 10.0.0.5/30
R1
Gi0/0 → 10.0.0.1/30
🔀 VLAN Configuration

VLANs were created on the switches:

VLAN 10 → MANAGEMENT
VLAN 20 → FINANCE
VLAN 30 → HR
VLAN 40 → SALES
VLAN 50 → IT
VLAN 60 → GUEST
VLAN 70 → SERVERS
VLAN 99 → NATIVE-MGMT
🔗 Trunk Configuration

Trunk links use:

802.1Q
Native VLAN 99

CoreSW1:

Fa0/1 → trunk
Fa0/2 → trunk
Fa0/3 → trunk

Allowed VLANs:

10,20,30,40,50,60,70,99

The trunk configuration was verified with:

show interfaces trunk
🧩 Access Port Configuration

AccessSW1 example:

Fa0/1 → Trunk
Fa0/2 → VLAN 10
Fa0/3 → VLAN 20

Current connected test PCs:

VLAN 10 PC
IP:      10.10.10.10
Mask:    255.255.255.0
Gateway: 10.10.10.1
VLAN 20 PC
IP:      10.10.20.10
Mask:    255.255.255.0
Gateway: 10.10.20.1
🖥️ SVI Configuration

CoreSW1 works as a Layer 3 switch.

SVI interfaces:

interface Vlan10
 ip address 10.10.10.1 255.255.255.0

interface Vlan20
 ip address 10.10.20.1 255.255.255.0

IP routing was enabled:

ip routing

Verification:

show ip interface brief

Result:

Vlan10 → 10.10.10.1 → up/up
Vlan20 → 10.10.20.1 → up/up
🔄 Inter-VLAN Routing

Inter-VLAN Routing was tested between:

VLAN 10
10.10.10.0/24

and

VLAN 20
10.10.20.0/24

Test:

ping 10.10.20.10

from VLAN 10 PC.

The ping was successful.

This confirmed that:

VLAN 10
   ↓
SVI
   ↓
Layer 3 Routing
   ↓
VLAN 20

is working correctly.

🛰️ OSPF Configuration

OSPF Process ID:

1

CoreSW1 Router ID:

3.3.3.3

R2 Router ID:

2.2.2.2

R1 Router ID:

1.1.1.1

OSPF Area:

Area 0
🔧 CoreSW1 OSPF
router ospf 1
 router-id 3.3.3.3
 network 10.0.0.4 0.0.0.3 area 0
 network 10.0.0.12 0.0.0.3 area 0
 network 10.10.10.0 0.0.0.255 area 0
 network 10.10.20.0 0.0.0.255 area 0
🔧 OSPF Network Design
R1
 |
 | 10.0.0.0/30
 |
R2
 |
 | 10.0.0.4/30
 |
CoreSW1
 |
 | 10.0.0.12/30
 |
CoreSW2

OSPF dynamically distributes the routing information between Layer 3 devices.

🔎 OSPF Verification

CoreSW1:

show ip ospf neighbor

Neighbor example:

Neighbor ID 2.2.2.2
State FULL/DR
Address 10.0.0.5

Another neighbor:

Neighbor ID 3.3.3.3
State FULL/DR
Address 10.0.0.13

This confirmed that OSPF neighbor relationships were successfully established.

🛣️ Routing Verification

R2 routing table successfully learned:

O 10.10.10.0/24 via 10.0.0.6
O 10.10.20.0/24 via 10.0.0.6

R1 routing table successfully learned:

O 10.10.10.0/24 via 10.0.0.2
O 10.10.20.0/24 via 10.0.0.2

This proves that VLAN networks are being propagated through OSPF.

🧪 End-to-End Testing

The following tests were successfully completed.

Test 1 — VLAN 10 Gateway
PC VLAN 10
10.10.10.10
       ↓
10.10.10.1

Result:

SUCCESS ✅
Test 2 — VLAN 20 Gateway
PC VLAN 20
10.10.20.10
       ↓
10.10.20.1

Result:

SUCCESS ✅
Test 3 — Inter-VLAN Routing
10.10.10.10
      ↓
10.10.10.1
      ↓
CoreSW1
      ↓
10.10.20.1
      ↓
10.10.20.10

Result:

SUCCESS ✅
Test 4 — OSPF Routing

R2 successfully learned:

10.10.10.0/24
10.10.20.0/24

Result:

SUCCESS ✅
Test 5 — End-to-End Routing

R1 successfully reached:

10.10.10.10
10.10.20.10

Ping result:

SUCCESS ✅

This confirms end-to-end connectivity across:

R1
 ↓
R2
 ↓
CoreSW1
 ↓
Access Switch
 ↓
VLAN
 ↓
PC
🧠 Technologies Practiced

During this stage of the project, the following technologies were practiced:

Ethernet
MAC Address
VLAN
Access Port
Trunk
802.1Q
Native VLAN
RSTP/STP
Layer 3 Switch
SVI
IP Routing
Inter-VLAN Routing
IPv4
Subnetting
OSPF
OSPF Neighbor
OSPF Area 0
Router ID
Dynamic Routing
Routing Table
CDP
Network Troubleshooting
Ping Testing
End-to-End Connectivity
🛠️ Troubleshooting Performed

During the implementation several configuration issues were identified and resolved.

Native VLAN mismatch

CDP reported:

%CDP-4-NATIVE_VLAN_MISMATCH

The issue was related to different native VLAN configurations on trunk links.

The solution was to use:

Native VLAN 99

on both sides of the trunk.

Trunk Encapsulation Issue

One interface returned:

Command rejected:
An interface whose trunk encapsulation is "Auto"
can not be configured to "trunk" mode.

The solution was:

switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 99

The interface then successfully became:

802.1q
trunking
Native VLAN 99
📊 Final Connectivity

The final tested path:

              OSPF
R1 ─────────── R2
                │
                │ OSPF
                │
             CoreSW1
             /  |  \
          TRUNK TRUNK TRUNK
           /     |     \
        SW1     SW2     SW3
         |
       VLANs
         |
        PCs

Successful end-to-end ping:

R1 → VLAN 10 PC ✅
R1 → VLAN 20 PC ✅
🎯 Project Status
Completed
 VLAN creation
 VLAN segmentation
 Access ports
 Trunk links
 Native VLAN 99
 RSTP/STP
 Core Layer 3 switching
 SVI
 IP routing
 Inter-VLAN Routing
 OSPF Area 0
 OSPF Neighbor formation
 OSPF route propagation
 End-to-End connectivity
 Troubleshooting
Next Steps

Planned future improvements:

 Configure SVI for VLAN 30
 Configure SVI for VLAN 40
 Configure SVI for VLAN 50
 Configure SVI for VLAN 60
 Configure SVI for VLAN 70
 DHCP Server
 DHCP Relay
 ACL
 SSH
 Port Security
 EtherChannel
 STP optimization
 BPDU Guard
 Network redundancy
 Enterprise security
 Cloud networking integration
📁 Packet Tracer File

The Cisco Packet Tracer topology is stored in this repository.

Example:

Enterprise-Bank-Network/
│
├── README.md
├── packet-tracer/
│   └── Enterprise-Bank-Network.pkt
│
├── configs/
│   ├── R1.txt
│   ├── R2.txt
│   ├── CoreSW1.txt
│   ├── CoreSW2.txt
│   ├── AccessSW1.txt
│   ├── AccessSW2.txt
│   └── AccessSW3.txt
│
└── screenshots/
    ├── topology.png
    ├── vlan.png
    ├── trunk.png
    ├── ospf-neighbor.png
    └── routing.png
👨‍💻 Author

Amir Adizov

Cisco Networking / Cloud Networking Learning Project

Technologies:

Cisco Packet Tracer
Cisco IOS
VLAN
802.1Q
STP/RSTP
SVI
Inter-VLAN Routing
OSPF
IPv4

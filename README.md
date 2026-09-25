<div align="center">

# 🌐 Enterprise IPv4 Network with OSPF & Automatic Failover

### A CCNA 200-301 portfolio project built and validated in Cisco Packet Tracer

[![Cisco](https://img.shields.io/badge/Cisco-CCNA%20200--301-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)](https://www.cisco.com/)
[![OSPF](https://img.shields.io/badge/Routing-OSPF%20Area%200-005073?style=for-the-badge)](https://www.cisco.com/)
[![IPv4](https://img.shields.io/badge/Addressing-IPv4%20%2B%20VLSM-2E7D32?style=for-the-badge)](https://www.cisco.com/)
[![Failover](https://img.shields.io/badge/Resilience-Redundant%20Core%20%2B%20Floating%20Static-F57C00?style=for-the-badge)](https://www.cisco.com/)
[![Packet Tracer](https://img.shields.io/badge/Lab-Cisco%20Packet%20Tracer-6A1B9A?style=for-the-badge)](https://www.netacad.com/courses/packet-tracer)

**Design → Configure → Verify → Break → Recover**

</div>

---

## 📌 Project Overview

This project demonstrates the design, configuration, and validation of a **small enterprise IPv4 network** using core **CCNA-level routing and network resilience concepts**.

The network connects **four user LANs** through a six-router enterprise topology and provides simulated internet access through a dedicated edge router. **OSPF Area 0** is used for dynamic routing, while a **redundant core path** provides automatic recovery when a primary link fails. Selected **floating static routes** are also configured as secondary routing paths.

The project is intentionally documented like a real network engineering lab: it includes the architecture, addressing plan, device configurations, verification commands, failure testing, and evidence of the results.

### 🎯 Problem Statement

> **How can an enterprise network maintain connectivity when a routing path or core link fails without requiring manual route changes?**

### 💡 Solution

- **OSPF Area 0** → primary dynamic routing
- **Redundant R2–R3/R4–R5 core** → alternate path
- **Floating static routes (AD 150)** → secondary backup paths
- **VLSM addressing** → efficient IPv4 allocation
- **Default-route propagation** → centralized internet exit through R2
- **Packet Tracer failover testing** → validates the design under failure

---

## ⭐ Project at a Glance

| Metric | Implementation |
|:---|:---|
| **Routers** | 6 enterprise routers + 1 simulated ISP |
| **User LANs** | 4 |
| **End-user PCs** | 12 |
| **Routing Protocol** | OSPFv2 — Area 0 |
| **Addressing** | IPv4 + VLSM |
| **LAN Subnets** | `/27` |
| **Point-to-Point Links** | `/30` |
| **Primary AD** | OSPF — `110` |
| **Backup AD** | Floating static — `150` |
| **Internet Edge** | R2 |
| **Failover Test** | R2 ↔ R3 link failure |
| **Observed Packet Loss** | 1 packet during the tested failover |
| **Simulation Tool** | Cisco Packet Tracer |

---

## 🖼️ Network Architecture

The architecture diagram gives a high-level view of the enterprise design before looking at the detailed Packet Tracer topology.

<div align="center">

![Enterprise IPv4 Network Architecture](images/enterprise-network-architecture.jpeg)

**Figure 1 — Enterprise IPv4 network architecture**

</div>

### 🧩 Architecture Roles

| Device / Role | Responsibility |
|---|---|
| **R1 — Access/LAN** | Connects the R1 user LAN to the enterprise core |
| **R2 — Internet Edge** | Central internet exit; originates the default route into OSPF |
| **R3 — Distribution** | Connects the R3 LAN and one side of the redundant core |
| **R4 — Distribution** | Connects the R4 LAN and the alternate side of the redundant core |
| **R5 — Core** | Provides the redundant connection between the distribution layer and R6 |
| **R6 — Access/LAN** | Connects the R6 user LAN to the core |
| **ISP — Simulation** | Represents an upstream internet provider |

### 🔄 High-Level Traffic Flow

```text
User LAN
   │
   ▼
Access Router
   │
   ▼
Distribution / Core
   │
   ├──────── Primary OSPF path
   │
   └──────── Alternate path after link failure
   │
   ▼
R2 Internet Edge
   │
   ▼
Simulated ISP
```

---

## 🖥️ Packet Tracer Implementation

The complete topology was implemented in **Cisco Packet Tracer** using:

- 6 × Cisco 2811 enterprise routers
- 1 × Cisco 2811 simulated ISP
- 4 × Cisco 2960-24TT switches
- 12 × PC-PT end devices
- Loopback interfaces for stable OSPF Router IDs
- IPv4 VLSM addressing
- OSPF Area 0
- Floating static backup routes

<div align="center">

![Packet Tracer Topology](images/packet-tracer-topology.png)

**Figure 2 — Complete Cisco Packet Tracer implementation**

</div>

---

## 🧠 Networking Concepts Demonstrated

| Skill Area | Demonstrated Implementation |
|---|---|
| **IPv4 Subnetting** | `/27` LANs and `/30` point-to-point links |
| **VLSM** | Efficient allocation from `10.0.0.0/24` |
| **OSPFv2** | Single-area OSPF Area 0 |
| **OSPF Neighbors** | Neighbor adjacency verification |
| **Dynamic Routing** | Automatic route learning and convergence |
| **Static Routing** | Floating static routes with AD 150 |
| **Administrative Distance** | OSPF `110` vs static `150` |
| **Redundancy** | Two core paths between R2 and R5 |
| **Default Routing** | R2 default route toward simulated ISP |
| **Failover Testing** | Link shutdown and connectivity validation |
| **Troubleshooting** | `show ip route`, `show ip ospf neighbor`, ping testing |
| **Network Documentation** | Architecture, addressing, configs, and test evidence |

---

## 🧭 IP Addressing Plan

### User LANs — `/27`

Each `/27` provides **30 usable IPv4 host addresses**.

| LAN | Network | Gateway | Switch | Hosts |
|---|---|---|---|---|
| **R1 LAN** | `10.0.0.0/27` | `10.0.0.1` | Switch4 | PC1 `.2`, PC2 `.3`, PC3 `.4` |
| **R3 LAN** | `10.0.0.32/27` | `10.0.0.33` | Switch5 | PC4 `.34`, PC5 `.35`, PC6 `.36` |
| **R4 LAN** | `10.0.0.64/27` | `10.0.0.65` | Switch6 | PC7 `.66`, PC8 `.67`, PC9 `.68` |
| **R6 LAN** | `10.0.0.96/27` | `10.0.0.97` | Switch7 | PC10 `.98`, PC11 `.99`, PC12 `.100` |

**LAN summary:** `10.0.0.0/25`

### Point-to-Point Links — `/30`

| Link | Subnet | Side A | Side B |
|---|---|---|---|
| R1 ↔ R2 | `10.0.0.128/30` | R1 `.129` | R2 `.130` |
| R2 ↔ R3 | `10.0.0.132/30` | R2 `.133` | R3 `.134` |
| R2 ↔ R4 | `10.0.0.136/30` | R2 `.137` | R4 `.138` |
| R3 ↔ R5 | `10.0.0.140/30` | R3 `.141` | R5 `.142` |
| R4 ↔ R5 | `10.0.0.144/30` | R4 `.145` | R5 `.146` |
| R5 ↔ R6 | `10.0.0.148/30` | R5 `.149` | R6 `.150` |
| R2 ↔ ISP | `203.0.113.0/30` | R2 `.1` | ISP `.2` |

---

## 🔁 Routing & Resilience Design

### Primary Routing

**OSPF Area 0** is the primary routing mechanism.

- OSPF AD: **110**
- Dynamic route discovery
- Neighbor adjacency between routers
- Automatic route recalculation after topology changes
- Equal-cost paths can be installed where available

### Backup Routing

Floating static routes use **AD 150**, making them less preferred than OSPF.

```text
OSPF route       → AD 110 → preferred
Floating static  → AD 150 → backup
```

This demonstrates the practical use of **Administrative Distance** in Cisco routing.

### Internet Routing

R2 acts as the network edge:

```text
R2 → 0.0.0.0/0 → 203.0.113.2 → ISP
```

R2 uses:

```text
default-information originate
```

to advertise the default route into OSPF.

---

## 📂 Repository Structure

```text
ccna-enterprise-network-ospf/
├── README.md
│
├── configs/
│   ├── R1.txt
│   ├── R2.txt
│   ├── R3.txt
│   ├── R4.txt
│   ├── R5.txt
│   ├── R6.txt
│   └── ISP.txt
│
├── images/
│   ├── enterprise-network-architecture.jpeg
│   ├── packet-tracer-topology.png
│   ├── ospf-neighbors.png
│   ├── routing-table.png
│   └── failover-test.png
│
└── packet-tracer/
    └── enterprise-network.pkt
```

---

## ⚙️ Device Configuration

Cleaned routing configurations are provided in the [`configs/`](configs/) directory.

Each file contains the important configuration elements for that device:

- Hostname
- Loopback interface
- Routed interfaces
- Interface descriptions
- OSPF configuration
- Floating static routes
- R2 default route where applicable

### Why include the configurations?

For a recruiter or technical reviewer, the repository provides more than a topology image — the **actual device configuration is available for inspection and reproduction**.

> **Note:** The configurations are intentionally cleaned of Packet Tracer boilerplate so the routing and addressing logic is easier to review.

---

## 🚀 How to Run the Lab

### 1. Open the project

Install **Cisco Packet Tracer** and open:

```text
packet-tracer/enterprise-network.pkt
```

### 2. Allow OSPF to converge

Wait for the router links and OSPF adjacencies to stabilize.

### 3. Verify OSPF

```text
show ip ospf neighbor
```

Expected result: OSPF neighbors should reach the **FULL** state.

### 4. Verify routing

```text
show ip route
show ip route ospf
```

### 5. Test connectivity

From an end device, test communication between different LANs:

```text
ping 10.0.0.98
```

### 6. Perform the failover test

Shut down the R2 ↔ R3 link and observe OSPF reconvergence through R4.

---

## ✅ Verification Evidence

### 1. OSPF Neighbor Adjacency

R2 establishes OSPF adjacencies with R1, R3, and R4.

<div align="center">

![OSPF Neighbor Verification](images/ospf-neighbors.png)

**Figure 3 — OSPF neighbor verification on R2**

</div>

---

### 2. Routing Table Verification

The R2 routing table shows connected routes, OSPF-learned routes, and the static default route toward the simulated ISP.

<div align="center">

![R2 Routing Table](images/routing-table.png)

**Figure 4 — R2 routing table**

</div>

---

## 🧪 Failover Test

The network was tested by intentionally shutting down the **R2 ↔ R3** link while continuously pinging from the R1 LAN to the R6 LAN.

### Test Procedure

1. Start a continuous ping from **PC1 → PC10**.
2. Shut down the **R2 Fa1/0** interface.
3. Observe the OSPF topology change.
4. Verify that traffic continues through the R4 path.
5. Restore the interface and confirm OSPF reconvergence.

### Observed Result

**One packet was lost during the tested link failure, after which connectivity resumed through the redundant path.**

<div align="center">

![OSPF Failover Test](images/failover-test.png)

**Figure 5 — Link failure, OSPF reconvergence, and routing-table verification**

</div>

### Before vs After

| Route | Healthy Network | R2 ↔ R3 Link Down |
|---|---|---|
| R3 LAN `10.0.0.32/27` | Via R3 | Reached through R4 |
| R6 LAN `10.0.0.96/27` | Equal-cost paths via R3/R4 | Via R4 |
| R3 ↔ R5 `10.0.0.140/30` | Via R3 | Via R4 |

**Key takeaway:** the failure was introduced deliberately, and the routing protocol recalculated the available path without requiring manual route replacement.

---

## 🔍 Key Cisco Commands Used

### OSPF

```text
router ospf 1
network 10.0.0.0 0.0.0.255 area 0
network 192.168.0.0 0.0.0.255 area 0
```

### Verify OSPF neighbors

```text
show ip ospf neighbor
```

### Verify OSPF routes

```text
show ip route ospf
```

### Verify complete routing table

```text
show ip route
```

### Floating static route

```text
ip route <network> <mask> <next-hop> 150
```

### Default route

```text
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

### Advertise default route through OSPF

```text
default-information originate
```

### Simulate link failure

```text
interface fa1/0
shutdown
```

### Restore the link

```text
interface fa1/0
no shutdown
```

---

## 💼 What This Project Demonstrates to Recruiters

This project is designed to demonstrate practical networking ability rather than only theoretical knowledge.

### Technical Skills

- Cisco IOS routing configuration
- OSPFv2
- IPv4 subnetting and VLSM
- Static and floating static routing
- Administrative Distance
- Default routing
- Redundant network design
- Route verification and troubleshooting
- Failure simulation and recovery validation
- Cisco Packet Tracer

### Engineering Approach

The project follows a simple engineering workflow:

```text
Requirements
     ↓
Network Design
     ↓
IP Address Planning
     ↓
Cisco Configuration
     ↓
Routing Verification
     ↓
Failure Simulation
     ↓
Recovery Validation
     ↓
Documentation
```

This makes the project useful as a **portfolio demonstration for entry-level Network Engineer, Network Support, NOC, and infrastructure-focused roles**.

---

## 🎯 Why the Design Matters

The project demonstrates several real-world networking principles:

**Availability**  
Redundant routing paths reduce the impact of a single core-link failure.

**Scalability**  
VLSM and a structured addressing plan make the network easier to expand.

**Automation**  
OSPF dynamically recalculates routes after topology changes.

**Troubleshooting**  
The lab includes commands and evidence for verifying neighbors, routes, connectivity, and failover.

**Documentation**  
The complete repository includes the topology, addressing plan, configurations, Packet Tracer file, and test evidence.

---

## 🔭 Future Enhancements

The current project focuses on routing and resilience. Possible next iterations include:

- Add **VLANs and inter-VLAN routing**
- Add **DHCP**
- Add **NAT/PAT** at the internet edge
- Add **extended and standard ACLs**
- Add **OSPF authentication**
- Configure **passive interfaces**
- Introduce **multi-area OSPF**
- Add **IPv6 and OSPFv3**
- Add **SSH and device hardening**
- Add **network monitoring and logging**

---

## 🎓 CCNA Concepts Covered

This project directly reinforces the following CCNA areas:

| CCNA Area | Topics Practiced |
|---|---|
| **Network Fundamentals** | IPv4, subnetting, VLSM, addressing |
| **Network Access** | LAN connectivity and switching |
| **IP Connectivity** | OSPF, static routing, default routing, AD |
| **IP Services** | Default-route propagation |
| **Security Fundamentals** | Foundation for future ACL/SSH hardening |
| **Automation & Operations** | Structured configuration and verification workflow |

---

## 🏁 Conclusion

This project demonstrates the complete lifecycle of building and validating a small enterprise IPv4 routing environment in Cisco Packet Tracer.

The network combines **OSPF Area 0, VLSM addressing, redundant core paths, floating static routes, default-route propagation, and controlled failover testing** into one practical lab. The R2 ↔ R3 link-failure test demonstrates that OSPF can reconverge through the alternate R4 path while maintaining end-to-end connectivity after the transition.

More importantly, the project goes beyond configuration by documenting **why the network was designed this way, how each device is configured, how the routing behavior is verified, and how failure recovery is tested**.

> **Design it. Configure it. Test it. Break it. Recover it. Document it.**

That workflow reflects the practical mindset expected in entry-level networking and infrastructure roles.

---

## 👤 Author

### **Amaanali Motiwala**
**B.Tech Cybersecurity | Aspiring Network Engineer | CCNA 200-301**

**Focus Areas:** Networking • Cloud Security • SOC • Network Security

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/amaanali-motiwala-67208628a/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github&logoColor=white)](https://github.com/AmaanaliMotiwala0109)
[![Email](https://img.shields.io/badge/Email-Contact-D14836?style=flat&logo=gmail&logoColor=white)](mailto:amaanalimotiwala@gmail.com)

---

<div align="center">

⭐ **If you found this project useful, consider giving it a star.**

</div>

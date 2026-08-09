# Enterprise Network Architecture 🌐

<p align="center">
  <strong>Enterprise Network Design & Simulation using Cisco Packet Tracer</strong><br>
  Multi-site routing, switching, security, services, and high availability
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white" alt="Cisco Packet Tracer">
  <img src="https://img.shields.io/badge/Networking-Enterprise-0A66C2?style=for-the-badge" alt="Enterprise Networking">
  <img src="https://img.shields.io/badge/Routing-OSPF%20%7C%20EIGRP%20%7C%20eBGP-6f42c1?style=for-the-badge" alt="Routing">
  <img src="https://img.shields.io/badge/Security-ACL%20%7C%20DHCP%20Snooping%20%7C%20Port%20Security-d73a49?style=for-the-badge" alt="Security">
</p>

## 📌 Project Overview

This project is a complete **multi-site enterprise network architecture and simulation** built with **Cisco Packet Tracer**. It connects a **Headquarters (HQ)**, **Engineering Campus**, and **Remote Campus** through an ISP-facing WAN while implementing real-world enterprise routing, switching, security, network services, and redundancy technologies.

The project focuses on building a network that is **scalable, redundant, segmented, secure, and verifiable** through practical configuration and connectivity tests.

### 🏢 Sites & Main Components

- **Headquarters (HQ)** with redundant core switches and a data-center/server segment.
- **Engineering Campus** with redundant distribution switches and segmented user networks.
- **Remote Campus** with its own core, VLANs, wireless clients, and WAN connectivity.
- **ISP/WAN edge** providing external routing and eBGP connectivity.
- **DHCP, DNS, and Database services** hosted in the HQ server segment.
- **Wireless access points and clients** distributed across the campuses.

![Complete Enterprise Network Topology](screenshots/topology/enterprise-network-topology.png)

*Complete multi-site enterprise topology including HQ, Engineering, Remote Campus, ISP, VLANs, servers, and wireless components.*

---

## 🧭 Table of Contents

- [Architecture](#-architecture)
- [Routing & WAN](#-routing--wan-connectivity)
- [Switching & VLANs](#-switching--vlans)
- [High Availability](#-high-availability--redundancy)
- [Network Security](#-network-security)
- [Network Services](#-network-services)
- [Testing & Verification](#-testing--verification)
- [Technologies](#-technologies--protocols)
- [Project Structure](#-project-structure)
- [Skills Demonstrated](#-skills-demonstrated)
- [Project Files](#-project-files)
- [Author](#-author)

---

## 🏗️ Architecture

The network follows a hierarchical and multi-site enterprise design with Layer 2 access connectivity, Layer 3 distribution/core functions, redundant gateways, and routed WAN links.

### Main architectural elements

- Multi-site campus architecture
- Dedicated VLANs for different departments and services
- Layer 3 SVIs for inter-VLAN routing
- Redundant core/distribution switches
- WAN routing between sites
- ISP-facing edge connectivity
- Centralized network services at HQ
- Wireless access at each campus
- Multiple paths for network resiliency

---

## 🔀 Routing & WAN Connectivity

The project implements multiple routing domains and demonstrates interaction between different routing protocols.

### OSPF

- OSPF dynamic routing
- OSPF-learned routes
- OSPF external Type-2 (`O E2`) routes
- Routing-table verification

![OSPF and EIGRP Routing Tables](screenshots/routing/ospf-and-eigrp-routing-tables.png)

### EIGRP

- EIGRP dynamic routing
- EIGRP-learned routes (`D`)
- EIGRP external routes (`D EX`)
- Routing-table verification

### eBGP

- eBGP connectivity between the Remote Campus edge and ISP
- BGP-learned routes (`B`)
- WAN/Internet-edge routing verification

![eBGP Routing Tables](screenshots/routing/ebgp-routing-tables-rc-and-isp.png)

### Route Redistribution

The routing tables provide evidence of **route redistribution between routing domains**, including OSPF external (`O E2`) and EIGRP external (`D EX`) routes.

The project therefore demonstrates the practical use of:

- OSPF
- EIGRP
- eBGP
- Route Redistribution
- Multi-domain routing
- WAN/ISP connectivity
- Routing-table verification

---

## 🔄 Switching & VLANs

### VLAN Segmentation

The enterprise network uses VLAN segmentation to separate users, departments, services, voice, wireless, and infrastructure traffic.

Examples include:

- **HQ:** VLANs 10–90, including HR (VLAN 80) and Servers (VLAN 60).
- **Engineering:** VLANs 110–160 for students, labs, staff, Wi-Fi, CCTV, and phones.
- **Remote Campus:** VLANs 210–240 for students, staff, labs, and Wi-Fi.

### SVI & Inter-VLAN Routing

**Switch Virtual Interfaces (SVIs)** are configured as Layer 3 gateways for local VLANs, enabling communication between different VLANs while maintaining logical network segmentation.

![HQ SVI, VLAN & HSRP Status](screenshots/switching/hq-core-svis-vlans-and-hsrp-status.png)

![Campus SVI, VLAN & HSRP Status](screenshots/switching/campus-svis-vlans-and-hsrp-status.png)

![Engineering SVI, VLAN & HSRP Status](screenshots/switching/engineering-distribution-svis-vlans-and-hsrp-status.png)

### 802.1Q Trunking

Trunk links carry multiple VLANs between switching devices and support the multi-VLAN campus architecture.

### EtherChannel / LACP

- EtherChannel configuration
- LACP negotiation
- Port-channel operation
- 802.1Q trunking over aggregated links
- Link redundancy and increased aggregate bandwidth

![LACP EtherChannel & Trunk Status](screenshots/switching/lacp-etherchannel-and-trunk-status.png)

### Rapid-PVST+

Rapid-PVST+ is used for Layer 2 loop prevention and faster spanning-tree convergence across the redundant switching topology.

---

## 🛡️ High Availability & Redundancy

The design uses several technologies together to reduce single points of failure.

### HSRP

**Hot Standby Router Protocol (HSRP)** provides redundant default gateways for VLANs using active/standby roles across the HQ core and Engineering distribution switch pairs.

The project verifies:

- HSRP active/standby states
- Virtual gateway addresses
- Redundant SVI gateways
- First-hop gateway resiliency

### EtherChannel / LACP

Aggregates multiple physical links into logical Port-channels, improving resiliency and maintaining connectivity if an individual member link fails.

### Rapid-PVST+

Provides per-VLAN loop prevention and rapid Layer 2 convergence.

### Overall redundancy design

- HSRP gateway redundancy
- LACP link redundancy
- Rapid-PVST+ loop prevention
- Redundant core/distribution paths
- Multiple routed paths between sites

---

## 🔐 Network Security

The project includes practical Layer 2 and Layer 3 security controls.

### Extended ACLs

A dedicated `DB-ACCESS` extended ACL protects the database server at `10.10.60.20`:

```text
10 permit ip 10.10.80.0 0.0.0.255 host 10.10.60.20
20 deny ip any host 10.10.60.20
30 permit ip any any
```

This policy:

- Allows the **HR subnet (VLAN 80 / 10.10.80.0/24)** to access the database server.
- Blocks other sources from accessing the database server.
- Allows unrelated traffic to continue normally.

![DB Server ACL Policy](screenshots/security/db-server-access-acl-policy.png)

![DB Server ACL Validation](screenshots/security/db-server-acl-validation.png)

The validation evidence demonstrates successful HR access and blocked Finance access to the protected database server.

### DHCP Snooping

DHCP snooping is used to build trusted DHCP bindings and help prevent unauthorized DHCP-server behavior on access networks.

### Port Security

Port-security controls are applied on access interfaces to restrict unauthorized endpoint access and provide Layer 2 access protection.

![Rapid-PVST+, DHCP Snooping & Port Security](screenshots/security/rapid-pvst-dhcp-snooping-and-port-security.png)

---

## 🌐 Network Services

### DHCP

Centralized DHCP pools provide IP addressing information to clients across the enterprise VLANs.

![DHCP Pools & Client Addressing](screenshots/services/dhcp-pools-and-client-addressing.png)

### DNS

DNS is implemented for internal name resolution, including the `university.local` domain shown in the verification evidence.

### Wireless Networking

Access points and wireless clients are included at HQ, Engineering, and the Remote Campus to represent wireless enterprise access.

### Database Services

The HQ data-center segment includes a database server at `10.10.60.20`, protected by the documented ACL policy.

### Inter-campus Connectivity

Connectivity between representative endpoints across the different campuses is validated using ping and DNS tests.

![DNS & Inter-campus Connectivity](screenshots/services/dns-and-intercampus-connectivity.png)

---

## 🧪 Testing & Verification

The network was validated through practical operational checks, including:

- `show ip route`
- OSPF route verification
- EIGRP route verification
- eBGP route verification
- External-route verification
- `show vlan brief`
- `show ip interface brief`
- `show standby brief`
- EtherChannel / LACP verification
- Trunk verification
- Rapid-PVST+ verification
- DHCP snooping binding verification
- Port-security verification
- ACL policy verification
- DHCP client addressing
- DNS resolution
- Inter-campus ping tests
- Positive and negative ACL connectivity tests

---

## 📊 Technologies & Protocols

| Category | Technologies |
|---|---|
| **Simulation** | Cisco Packet Tracer |
| **Routing** | OSPF, EIGRP, eBGP |
| **Routing Integration** | Route Redistribution |
| **Layer 3 Switching** | SVI, Inter-VLAN Routing |
| **Segmentation** | VLANs, 802.1Q Trunking |
| **High Availability** | HSRP |
| **Link Aggregation** | EtherChannel, LACP |
| **Spanning Tree** | Rapid-PVST+ |
| **IP Services** | DHCP, DNS |
| **Security** | Extended ACLs, DHCP Snooping, Port Security |
| **Wireless** | Access Points, Wireless Clients |
| **WAN** | Inter-site Routing, ISP Connectivity, eBGP |

---

## 📁 Project Structure

```text
Enterprise-Network-Architecture/
├── Abdelrahman-Enterprise-Network.pkt
├── README.md
└── screenshots/
    ├── topology/
    │   └── enterprise-network-topology.png
    ├── routing/
    │   ├── ebgp-routing-tables-rc-and-isp.png
    │   └── ospf-and-eigrp-routing-tables.png
    ├── switching/
    │   ├── campus-svis-vlans-and-hsrp-status.png
    │   ├── engineering-distribution-svis-vlans-and-hsrp-status.png
    │   ├── hq-core-svis-vlans-and-hsrp-status.png
    │   └── lacp-etherchannel-and-trunk-status.png
    ├── security/
    │   ├── db-server-access-acl-policy.png
    │   ├── db-server-acl-validation.png
    │   └── rapid-pvst-dhcp-snooping-and-port-security.png
    └── services/
        ├── dhcp-pools-and-client-addressing.png
        └── dns-and-intercampus-connectivity.png
```

---

## 📸 Verification Screenshots

| Category | Evidence |
|---|---|
| **Topology** | Complete enterprise topology |
| **Routing** | OSPF, EIGRP, eBGP and external-route evidence |
| **Switching** | VLANs, SVIs, HSRP, EtherChannel and trunking |
| **Security** | ACLs, DHCP Snooping and Port Security |
| **Services** | DHCP, DNS and inter-campus connectivity |

All verification screenshots are organized under the `screenshots/` directory for easier navigation.

---

## 💡 Skills Demonstrated

This project demonstrates practical skills in:

- Enterprise Network Architecture
- Cisco Routing & Switching
- OSPF
- EIGRP
- eBGP
- Route Redistribution
- VLAN Design & Segmentation
- SVI Configuration
- Inter-VLAN Routing
- HSRP
- EtherChannel / LACP
- 802.1Q Trunking
- Rapid-PVST+
- DHCP & DNS
- Extended ACLs
- DHCP Snooping
- Port Security
- Wireless Networking
- WAN Connectivity
- Network Troubleshooting
- Connectivity Testing
- Network Redundancy & High Availability

---

## 📦 Project Files

- **`Abdelrahman-Enterprise-Network.pkt`** — Complete Cisco Packet Tracer simulation.
- **`README.md`** — Full project documentation.
- **`screenshots/`** — Organized topology and configuration verification evidence.

The Packet Tracer `.pkt` file is preserved as the source-of-truth simulation artifact.

---

## 👨‍💻 Author

### Abdelrahman Mohamed

**Electrical Engineering Student | Telecommunications & Electronics**

Interested in **Computer Networks, Network Engineering, Routing & Switching, and Enterprise Infrastructure**.

---

⭐ If you find this project useful, feel free to star the repository.

---

> **Note:** This documentation describes the technologies and outcomes evidenced by the Packet Tracer project and the included verification screenshots. It avoids claiming configuration details that are not directly supported by the available project evidence.

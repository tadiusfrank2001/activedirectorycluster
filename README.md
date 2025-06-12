# 🛡️ Active Directory Cluster with NAT Routing, DHCP, and Domain Integration

Welcome to the **Active Directory Cluster**, a hands-on project designed to simulate a functional Windows Server 2022 environment that manages Windows 10 Pro workstations within a domain. This lab combines core enterprise infrastructure components such as **Active Directory**, **DHCP**, **Remote Access (NAT Routing)**, and **multi-NIC networking** using UTM virtual machines on a local host.

---

## 🎯 Project Objectives

- Deploy a **Windows Server 2022** VM (DC1) as a **multi-role server** running:
  - Active Directory Domain Services (ADDS)
  - DHCP Server
  - Remote Access Services (NAT)
- Route internet traffic from a **private LAN subnet** to the public network via DC1
- Dynamically assign IPs to internal domain workstations
- Join **Windows 10 Pro** VMs to the domain and authenticate domain users
- Build and manage a **self-contained Active Directory lab** suitable for testing and learning

---

## 🧠 Skills Demonstrated

- Windows Server 2022 Installation & Configuration
- Multi-NIC Networking in UTM
- Active Directory Domain Services setup
- DHCP Server configuration with custom IP scopes
- Remote Access (Routing and NAT) for internet gateway
- Domain Join and User Authentication on Workstations
- Basic PowerShell and Windows CLI Networking Tools

---

## 🧩 Core Components

| Component     | Role                                                       |
|---------------|-------------------------------------------------------------|
| **DC1**        | Windows Server 2022 VM - Domain Controller for tfhomelab.local domain, DHCP, NAT Router |
| **Client1**    | Windows 10 Pro VM - Domain-joined workstation               |
| **Network**    | UTM-based dual-NIC system with NAT and internal LAN         |

---

## 🖥️ Network Design

The domain environment is built on an **isolated LAN** behind a **Windows Server 2022 router**. This allows the internal subnet to access the internet through NAT while maintaining domain-level management and IP assignment.

| NIC Name | UTM Network | IP Address        | Purpose                      |
|----------|-------------|-------------------|-------------------------------|
| `PUBLIC` | Shared NAT  | 10.0.2.x (static) | Internet access via host NAT |
| `PRIVATE`| Emulated LAN| 192.168.64.100    | Internal LAN, DHCP, ADDS     |


![Domain_TOPO](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/AD_network_topology.png)
---


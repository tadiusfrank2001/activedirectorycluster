# 🛡️ Active Directory Cluster with NAT Routing, DHCP, and Domain Integration

Welcome to the **Active Directory Cluster **, a hands-on project designed to simulate a functional Windows Server 2022 environment that manages Windows 10 Pro workstations within a domain. This lab combines core enterprise infrastructure components such as **Active Directory**, **DHCP**, **Remote Access (NAT Routing)**, and **multi-NIC networking** using UTM virtual machines on a local host.

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

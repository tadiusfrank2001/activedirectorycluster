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

## ⚙️ Core Components Configuration

Each component below has its own in-depth installation and configuration guide in the [`/docs`](./docs/) folder.

### 🖥️ [Windows Server 2022 - DC1](./docs/dc1_install.md)
- Installed via UTM with two network adapters
- Renamed network adapters to `PUBLIC` and `PRIVATE`
- Set static IPs for both interfaces
- Assigned DC1 the role of router and DNS/DHCP provider
- Used `sysprep` to clone image before domain setup

### 🏢 [Active Directory Domain Services (ADDS)](./docs/adds_config.md)
- Promoted DC1 to Domain Controller (`tfhomelab.local`)
- Created OUs and users (e.g., `tad.franco`)
- Enabled centralized login for domain-joined workstations

### 🌐 [Remote Access Services (Routing and NAT)](./docs/ras_config.md)
- Installed Routing and Remote Access (RRAS)
- Configured NAT on `PUBLIC` adapter
- Created static route for internal LAN: `192.168.64.0/24`

### 📦 [DHCP Server](./docs/dhcp_config.md)
- Created custom DHCP scope: `192.168.64.10–20`
- Set gateway as `192.168.64.100` (DC1)
- DNS servers: `127.0.0.1`, `8.8.8.8` (fallback)

### 🧍 [Windows 10 Pro Workstation(s)](./docs/workstation_join.md)
- Installed with UTM using Pro edition (not Home)
- Joined to domain using domain admin credentials
- Verified DHCP lease and internet access via NAT
- Successfully logged in with AD user (e.g., `tad.franco@corp.local`)

---
## 🔍 Reflections

### What This Teaches About Self-Hosting

This lab provides hands-on experience in building and managing a mini-enterprise network fully hosted by you, without relying on external cloud services. Key lessons include:

- **Control Over Infrastructure**  
  Setting up and managing Active Directory, DNS, DHCP, and NAT routing services gives you full control of your network environment, similar to how organizations self-manage their internal IT.

- **Private Networking and Isolation**  
  Using dual network interfaces and virtualized environments to create segmented public and private networks demonstrates how to isolate services and devices, a critical concept in self-hosting.

- **Hosting Core Identity Services**  
  Installing Active Directory Domain Services (ADDS) teaches how to centralize user authentication and access management on your own hardware (**IAM**).

- **DHCP and DNS Configuration**  
  Running your own DHCP and DNS servers shows how to dynamically assign IP addresses and resolve domain names internally, independent of ISPs or cloud providers.

- **Routing and NAT Setup**  
  Configuring Remote Access Services (RAS) with NAT highlights how to securely route traffic between private networks and the internet, an essential skill in self-hosted labs and production environments.

- **Domain Workstation Integration**  
  Joining workstations to your domain illustrates centralized management of users and devices, improving security and administration.

- **Efficient Lab Replication**  
  Using Sysprep to clone server images teaches how to efficiently replicate and scale your lab infrastructure.

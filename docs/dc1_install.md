# 🖥️ Windows Server 2022 - DC1 Setup

This guide walks through the installation and configuration of the Windows Server 2022 machine that acts as the **Domain Controller (DC1)** for the `tfhomelab` Active Directory domain. It is configured as a **router**, **DHCP server**, and **Active Directory Domain Services (ADDS)** provider.

---

## 📡 Network Design Overview

In our topology, the DC1 server is configured with two NICs:

- **PUBLIC NIC** (UTM: "Shared Network") — connects to the internet via NAT
- **PRIVATE NIC** (UTM: "Emulated LAN") — connects to the isolated LAN with domain clients

This setup allows DC1 to:

- Act as a **gateway** and **DHCP server** for the internal subnet
- Route traffic from internal domain workstations to the internet
- Provide domain authentication and management via ADDS

## ⚙️ Initial Windows Server VM Configuration

### 🔧 VM Creation in UTM

1. Create a new VM in UTM using the **Windows Server 2022 ISO**
2. Add **two network adapters**:
   - `Shared Network` (NAT) – for internet access
   - `Emulated LAN` – for internal network
3. Enable **randomized MAC address** for both adapters (optional but recommended, each NIC should have unique mac addresses)

### 🔹 Base Windows Configuration

- Set your **time zone** correctly
- Pin **Command Prompt** and **PowerShell** to the taskbar
- Run `ipconfig` in the terminal to identify each adapter:
  - `10.0.X.X` = Public NAT adapter
  - `192.168.X.X` = Private LAN adapter

![NIC config](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/PUBLIC_PRIVATE_NIC_CONFIG.png)

### 🧲 Verify Internet Access

Ensure the VM shows no "globe" icon in the network tray. Run:

```bash
ping 8.8.8.8
```

### 🧫 Run Sysprep for Cloning

Before configuring as a Domain Controller, sysprep the VM to allow clean clones if needed:

```bash
C:\Windows\System32\Sysprep\sysprep.exe
```

- Select: **Enter System Out-of-Box Experience (OOBE)**
- Check: **Generalize**
- Shutdown: **Shutdown**

---

## 🛠️ Rename and Configure Network Interfaces

### Rename Adapters

1. Go to **Control Panel > Network and Sharing > Change adapter settings**
2. Rename:
   - Public adapter → `PUBLIC`
   - Private adapter → `PRIVATE`

---

### Assign Static IPs

**For PUBLIC Adapter** (NAT):

- IP Address: `10.0.2.100` *(example)*
- Subnet Mask: auto-filled
- Default Gateway: `10.0.2.1`
- DNS:
  - `127.0.0.1`
  - `8.8.8.8`

**For PRIVATE Adapter** (LAN):

- IP Address: `192.168.64.100`
- Subnet Mask: `255.255.255.0`
- No default gateway
- DNS: `127.0.0.1` only

### Disable IPv6

In each adapter's properties, uncheck **Internet Protocol Version 6 (TCP/IPv6)**.

### Set Interface Metric (Private Before Public)

Run PowerShell as Administrator:

```powershell
Get-NetIPInterface
```

Make sure the **PRIVATE** adapter has a lower interface metric than PUBLIC. If not, manually set:

```powershell
Set-NetIPInterface -InterfaceAlias "PRIVATE" -InterfaceMetric 10
Set-NetIPInterface -InterfaceAlias "PUBLIC" -InterfaceMetric 20
```

---

Your server:

- Is clean and prepped from a sysprep image
- Has networking correctly configured
- Is ready to install ADDS and be promoted to a Domain Controller

> Continue to [ADDS Setup & Domain Promotion](./adds_config.md)



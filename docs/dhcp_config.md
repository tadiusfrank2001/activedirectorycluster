# 📦 DHCP Server Configuration (DC1)

The DHCP role on **DC1** provides automatic IP address assignment for devices within the internal LAN (`192.168.64.0/24`). This ensures that domain-joined workstations receive consistent, routable addresses and can access the internet via the NAT-configured DC1.

---

## 🛠️ Installing the DHCP Role

1. Open **Server Manager**
2. Go to **Manage > Add Roles and Features**
3. Choose:
   - **Role-based or feature-based installation**
   - Select **DHCP Server**
4. Complete the installation
5. Restart the server if not done automatically

![DHCP Wiz Post Download](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/DHCP_FEATURE_DOMAIN_DASHBOARD.png)

---

## 🌐 DHCP Scope Configuration

After installation:

1. Go to **Tools > DHCP**
2. Expand your server > **IPv4**
3. Right-click **IPv4** > Select **New Scope**
4. Follow the New Scope Wizard:

---

### 📋 Scope Details

- **Scope Name**: LAN Scope
- **IP Range**:  
  - Start: `192.168.64.10`  
  - End: `192.168.64.20`  
- **Subnet Mask**: `255.255.255.0`
- **Lease Duration**: Default (8 days or customize as needed)

![DHCP Config](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/DHCP_SERVER_SCOPE_CONFIGS.png)



### 🚪 Default Gateway (Router)

Add your **DC1 private IP** as the default gateway (router) for clients:

- **Router IP**: `192.168.64.100`  
- (Optional) Add public interface IP if needed: `10.0.2.100`

### 🌐 DNS Server Settings

Add DNS entries for domain resolution:

- `127.0.0.1` (Loopback / Local DNS)
- `192.168.64.100` (PRIVATE NIC)
- `10.0.2.100` (PUBLIC NIC)
- `8.8.8.8` (Google DNS — failsafe)

![DNS Config](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/DHCP_DNS_CONFIG.png)

## 🔁 Final Steps

Once scope is created:

- **Activate the scope** if not already
- Restart DHCP Server tasks to apply all configurations:

```powershell
Restart-Service DHCPServer

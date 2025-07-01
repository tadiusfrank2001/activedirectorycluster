## 🌐 Remote Access Services (Routing and NAT)

The **Routing and Remote Access Service (RRAS)** was installed on DC1 to enable NAT and internal routing between the PRIVATE (LAN) and PUBLIC (Internet) network interfaces.

---

### 🔧 RRAS Installation

1. Open **Server Manager**
2. Go to **Manage > Add Roles and Features**
3. Select:
   - **Role-based or feature-based installation**
   - Add **Remote Access**
   - On the role services screen, check:
     - **Routing**
4. Complete the wizard and **restart** the server if not done automatically

> 🖼️ ![RRAS Install Wizard](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/RAS_FEATURE_DOWNLOAD.png)


---

### ⚙️ RRAS Configuration

1. After reboot, go to **Tools > Routing and Remote Access**
2. Right-click the server name (e.g., `DC1`) and select **Configure and Enable Routing and Remote Access**
3. Follow the wizard:
   - Choose **Network Address Translation (NAT)**
   - Select the **PUBLIC adapter** as the interface connected to the internet
   - Enable NAT on this interface
   - Confirm and finish the setup

---

### 🛣️ Add Static Route for LAN

To allow proper routing from the LAN to the internet:

1. In **RRAS**, expand **IPv4 > Static Routes**
2. Right-click and choose **New Static Route**
3. Fill in:
   - **Destination**: `192.168.64.0`
   - **Network mask**: `255.255.255.0`
   - **Gateway**: Leave blank (or use your PRIVATE adapter if required)
   - **Interface**: Select the **PRIVATE adapter**

> 🖼️ ![Static Route Config](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/RAS_FEATURE_DOMAIN_DASHBOARD.png)

---

### 🔁 Restart RRAS

After configuration:

```powershell
Restart-Service RemoteAccess

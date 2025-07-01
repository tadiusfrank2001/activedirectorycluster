# 🛡️ Promoting Windows Server 2022 to Domain Controller - DC1

This document details the step-by-step configuration to promote a sysprepped Windows Server 2022 VM into a fully functional **Domain Controller (DC1)** for the `tfhomelab` domain.


## 🛠️ Step-by-Step ADDS Installation & Configuration

### 1. Install Active Directory Domain Services

- Open **Server Manager**
- Click on `Manage` → `Add Roles and Features`
- Proceed through the wizard:
  - Role-based installation
  - Select local server
  - **Select: Active Directory Domain Services**
  - Add required features when prompted

> 🖼️ ![ADDS Role Selection](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/DC1_ADDS_DOWNLOAD.png)

- Continue → Confirm → Install

> 💡 The server may require a restart after installation.



- Set DSRM password (Directory Services Restore Mode)
- Accept default NetBIOS name: `TFHOMELAB`
- Leave paths as default (unless you need custom ones)
- Review and validate

> 💡 Ignore warnings about DNS delegation if they appear

- Click **Install**

- The server will **restart automatically** after promotion.

---

### 3. Post-Promotion Verification

Once the server reboots:

- Log in with the domain admin account (e.g., `TFHOMELAB\Administrator`)
- Open **Server Manager**:
  - ADDS and DNS roles should be visible
- Open `cmd` or `PowerShell` and run:

```powershell
Get-ADDomain
Get-ADForest
```
🖼️ ![ADDS Dash](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/TFHOMELAB_DOMAIN_DASHBOARD.png)


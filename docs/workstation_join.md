# 🧍 Windows 10 Pro Workstation Setup (Client1)

This document walks through the setup and domain-joining of a **Windows 10 Pro** machine, referred to as `Client1`. Once joined, this machine can log in using **Active Directory (AD)** credentials created and managed by the **DC1** server.

---

## 🖥️ Installing Windows 10 Pro in UTM

1. Create a new VM using the **Windows 10 Pro ISO**
2. ⚠️ **Do NOT install Home Edition** – choose **Pro** so you can join a domain
3. Set up with:
   - **Emulated LAN adapter** (to match DC1’s PRIVATE NIC)
4. Use any temporary local admin credentials during install

## 🌐 Set DNS to Point to DC1

To allow the client to find and join the domain, you must point its DNS to the Domain Controller (DC1):

1. Open **Control Panel → Network and Internet → Network Connections**
2. Right-click your active network adapter → **Properties**
3. Select **Internet Protocol Version 4 (TCP/IPv4)** → Click **Properties**
4. Choose:  
   ✅ **Use the following DNS server address**

- Preferred DNS: `192.168.64.100` *(your DC1 private IP)*
- Alternate DNS: *(leave blank or add fallback if needed)*

Click **OK** and close.


---

## 🧑‍💻 Join the Domain

1. Go to **This PC → Properties**
2. Click **Advanced system settings**
3. Under **Computer Name**, click **Change**
4. Rename computer to `Client1`
5. Select:  
   ✅ **Member of a Domain**  
   ➡️ Enter domain name (e.g., `tfhomelab.local`)

You will be prompted for domain admin credentials. Enter the **username and password** used to set up **DC1**.

After success, the system will confirm domain join and reboot.


---

## 🔄 Verify Network + Domain Join

### 🧪 Ping Tests

With both **DC1** and **Client1** running:

1. Open CMD on Windows 10:
   ```bash
   ping 192.168.64.100

![PING Client to DHCP](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/PING_CLIENT_TO_DHCP_SERVER.png)

---

# 🔐 Create a Domain User: `tad.franco`

## 👨‍💼 Back on DC1:

1. Open **Server Manager → Tools → Active Directory Users and Computers**
2. Right-click `tfhomelab.local` → **New → Organizational Unit (OU)**  
   ➡️ **Name it**: `Users`
3. Right-click the `Users` OU → **New → User**

---

### 👤 User Details:

- **First Name**: Tad  
- **Last Name**: Franco  
- **Username**: `tad.franco`

---

### 🔐 Password Options:

- Set a strong password
- ✅ Uncheck **"User must change password at next login"**
- ✅ Check **"Password never expires"**


## 🛡️ Make Tad Franco a Domain Admin (Optional, but i did for go pratice we do not want to be in root user all the time here!)

1. In **AD Users and Computers**, go to the **Users** container
2. Right-click **Domain Admins** → **Properties → Members**
3. Click **Add**, search for `tad.franco`, and add the user

## 🔐 Log in from `Client1`

1. Reboot **Windows 10 (Client1)**
2. At the login screen, click **Other User**
3. Enter credentials:

```plaintext
Username: tfhomelab\tad.franco  
Password: **********
```
![USER LOGGED INTO DOMAIN](https://github.com/tadiusfrank2001/activedirectorycluster/blob/main/img/AD_User_Logged_In.png)

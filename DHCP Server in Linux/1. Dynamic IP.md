# **🌐 What is a DHCP Server?**  

A **DHCP (Dynamic Host Configuration Protocol) Server** is a network server that **automatically assigns IP addresses** and other network settings (such as subnet mask, gateway, and DNS) to devices on a network. This eliminates the need for manual IP configuration on client devices.  

---

# **⚙️ How DHCP Works?**  
When a device (client) connects to a network, it follows these steps to obtain an IP address from the **DHCP server**:  

1️⃣ **DHCP Discovery** (`DHCPDISCOVER`) – 📢 The client sends a broadcast request to find a DHCP server.  
2️⃣ **DHCP Offer** (`DHCPOFFER`) – 📩 The DHCP server responds with an available IP address and network settings.  
3️⃣ **DHCP Request** (`DHCPREQUEST`) – 📡 The client requests the offered IP address.  
4️⃣ **DHCP Acknowledgment** (`DHCPACK`) – ✅ The DHCP server confirms the assignment, and the client starts using the IP address.  

---

# **🔑 Key Features of DHCP Server**  
✅ **Automatic IP Assignment** – Reduces manual IP configuration.  
✅ **Prevents IP Conflicts** – Ensures each device gets a unique IP.  
✅ **Centralized Network Management** – Admins can control IP address distribution from a single server.  
✅ **IP Lease Management** – Assigns temporary IPs, reclaiming unused ones.  
✅ **Supports Reserved (Static) IPs** – Assigns permanent IPs to specific devices based on their MAC address.  

---

# **📍 Where is DHCP Used?**  
🏠 **Home Networks** – Routers use DHCP to assign IPs to connected devices.  
🏢 **Enterprise Networks** – Manages thousands of devices efficiently.  
🌍 **ISP Networks** – Assigns public IPs dynamically to customer devices.  

---

# **📊 Comparison of DHCP Server on Kali Linux, Debian, and CentOS**

| Feature | Kali Linux | Debian | CentOS |
|---------|------------|--------|--------|
| **Base OS** | Debian-based | Debian-based | RHEL-based |
| **Package Manager** | `apt` | `apt` | `yum` or `dnf` |
| **DHCP Server Package** | `isc-dhcp-server` | `isc-dhcp-server` | `dhcp-server` |
| **Config File Location** | `/etc/dhcp/dhcpd.conf` | `/etc/dhcp/dhcpd.conf` | `/etc/dhcp/dhcpd.conf` |
| **Firewall Tool** | `ufw` or `iptables` | `ufw` or `iptables` | `firewalld` or `iptables` |
| **Service Management** | `systemctl` | `systemctl` | `systemctl` |
| **Security Focus** | Penetration Testing | General Use | Enterprise Servers |
| **Common Use Case** | Ethical Hacking, Security Research | General Server & Workstation Use | Enterprise, Data Centers |

---

# **🛠 DHCP Server Installation & Configuration Guide (Debian-Based & CentOS)**  

## **1️⃣ Update System Packages**  
📂 Update package list to ensure you get the latest versions:  
- **Debian/Kali Linux:**
```bash
apt update &&  apt upgrade -y
```
- **CentOS:**
```bash
 yum update -y
```

---

## **2️⃣ Install DHCP Server Package**  
📥 Install the DHCP server package:  
- **Debian/Kali Linux:**
```bash
apt install isc-dhcp-server -y
```
- **CentOS:**
```bash
yum install dhcp-server -y
```

---

## **3️⃣ Verify Installation**  
🔍 Check if the DHCP package is installed:  
- **Debian/Kali Linux:**
```bash
dpkg -l | grep isc-dhcp-server
```
- **CentOS:**
```bash
rpm -qa | grep dhcp-server
```

---

## **4️⃣ List Configuration Files for DHCP Server**  
📜 Find the DHCP configuration files:  
- **Debian/Kali Linux:**
```bash
dpkg -L isc-dhcp-server
```
- **CentOS:**
```bash
rpm -ql dhcp-server
```

---

# **📑 DHCP Server Configuration**  

## **5️⃣ Edit DHCP Configuration File**  
✏️ Open the DHCP configuration file for editing:  
- **Debian/Kali Linux:**
```bash
 nano /etc/dhcp/dhcpd.conf
```
- **CentOS:**
```bash
 nano /etc/dhcp/dhcpd.conf
```

📝 **Sample Configuration File:**  
```bash
# DHCP Server Configuration File
# See /usr/share/doc/dhcp-server/dhcpd.conf.example
# See dhcpd.conf(5) man page for details

authoritative;

# Specify network address and subnet mask
subnet 192.168.88.0 netmask 255.255.255.0 {
    
    # Specify the range of lease IP addresses
    range 192.168.88.100 192.168.88.200;
    
    # Specify default gateway
    option routers 192.168.88.1;
    
    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    
    # Specify broadcast address
    option broadcast-address 192.168.88.255;
    
    # Default lease time (in seconds)
    default-lease-time 600;
    
    # Max lease time (in seconds)
    max-lease-time 7200;
}
```

---

# **⚡ Start and Manage DHCP Service**  

🔍 **Check Service Status**  
### Debian/Kali Linux
```bash
 systemctl status isc-dhcp-server  
```
 ### CentOS
```bash
systemctl status dhcpd  
```
🔄 **Restart DHCP Service**  
### Debian/Kali Linux
```bash
 systemctl restart isc-dhcp-server  
```
### CentOS
```bash
systemctl restart dhcpd 
```
🚀 **Enable Service to Start at Boot**  
### Debian/Kali Linux
```bash
 systemctl enable isc-dhcp-server  
```
### CentOS
```bash
 systemctl enable dhcpd
```

---

# **🛡️ Update Firewall Rules**  

🚪 **Allow DHCP traffic through the firewall:**  
- **Debian/Kali Linux (UFW):**
```bash
 ufw allow 67/udp
```
- **CentOS (Firewalld):**
```bash
 firewall-cmd --permanent --add-port=67/udp
```
### Reload Firewall Service
```bash
 firewall-cmd --reload
```

🔍 **Check if the DHCP port is open:**  
```bash
 netstat -nltup | grep dhcp
```

---

# **📑 Check DHCP Leases**  
🔍 Display current DHCP leases:  
### Debian/Kali Linux/CentOS
```bash
cat /var/lib/dhcp/dhcpd.leases  
```

---

# **✅ DHCP Client Configuration (Windows/Linux)**  

### **Windows Client**  
```powershell
ipconfig /release
```
```bash
ipconfig /renew
```
```bash
ipconfig
```

### **Linux Client**  
```bash
 dhclient -r &&  dhclient
```
```bash
ip a
```


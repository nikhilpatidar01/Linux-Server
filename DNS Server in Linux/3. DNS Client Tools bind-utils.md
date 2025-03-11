# 🖥️ **DNS Client Tools & Commands in Linux**  

**DNS client tools** help users query and manage DNS configurations. These tools are part of the **BIND utilities** package, which provides various utilities for DNS lookups, debugging, and updates.  

---

## **📌 Checking & Installing BIND Utilities**  

### ✅ **Check if BIND is Installed**
```sh
rpm -qa | grep bind
```
🔹 This command lists all installed BIND packages.  

### ✅ **Check if `bind-utils` is Installed**
```sh
rpm -qa | grep bind-utils
```
🔹 This checks if **BIND utilities** (like `dig`, `nslookup`, `host`) are installed.  

### ✅ **Install BIND Utilities (`bind-utils`)**
```sh
yum install bind-utils -y
```
🔹 This command installs **BIND utilities** on **CentOS/RHEL** systems.  
🔹 On **Debian/Ubuntu**, use:
```sh
sudo apt install dnsutils -y
```

---

## **📁 Listing Installed BIND Utility Files**  

### ✅ **List All Files Installed by `bind-utils`**
```sh
rpm -ql bind-utils
```
🔹 Displays all files installed by the **bind-utils** package.  

### ✅ **List Configuration Files for `bind-utils`**
```sh
rpm -qc bind-utils
```
🔹 Shows configuration files related to `bind-utils`.  

### ✅ **List Documentation Files for `bind-utils`**
```sh
rpm -qd bind-utils
```
🔹 Lists documentation files for reference.  

---

## **🔧 Common DNS Client Tools in `bind-utils`**  

| **Tool** | **Purpose** | **Location** |
|----------|------------|-------------|
| `dig` | Advanced DNS query tool | `/usr/bin/dig` |
| `nslookup` | Legacy DNS lookup tool | `/usr/bin/nslookup` |
| `host` | Simple DNS lookup tool | `/usr/bin/host` |
| `mdig` | Multi-query version of `dig` | `/usr/bin/mdig` |
| `delv` | DNS lookup and validation tool | `/usr/bin/delv` |
| `nsupdate` | Dynamic DNS update utility | `/usr/bin/nsupdate` |

---

## **📌 How to Use DNS Client Tools?**  

### 🔹 **Using `dig` (Domain Information Groper)**
```sh
dig example.com
dig example.com MX   # Get mail server records
dig example.com ANY  # Fetch all available DNS records
```

### 🔹 **Using `nslookup`**
```sh
nslookup example.com
nslookup example.com 8.8.8.8   # Query a specific DNS server
```

### 🔹 **Using `host`**
```sh
host example.com
host -t MX example.com   # Fetch mail records
```

### 🔹 **Using `nsupdate` (Dynamic DNS Updates)**
```sh
nsupdate -k /etc/rndc.key
```

---


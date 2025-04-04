  

## **🖥️ What is a DNS Server?**  
A **DNS (Domain Name System) server** translates human-readable domain names (like `example.com`) into IP addresses (like `192.168.1.1`). This is essential for accessing websites and online services.  

---

# ** 🌐 Types of DNS Servers 🌐**  

### **1️⃣ Non-Authoritative (Recursive) Nameserver 🔄**  
A **recursive DNS server** does **not** store original DNS records but finds answers by querying other DNS servers. It follows these steps:  
1. Receives a query for `example.com`.  
2. Asks the **root server** → then a **TLD server** (`.com` nameserver) → then the **authoritative server**.  
3. Retrieves the IP address and returns it to the user.  

✅ **Example: Testing DNS resolution using Google’s recursive nameserver**  
```sh
dig example.com @8.8.8.8
```


### ** 1. Caching Nameserver 🏪**  
A **caching DNS server** temporarily stores previously resolved queries. This speeds up future queries and reduces the load on authoritative servers.  

🔹 If someone queries `example.com`, the caching server stores the result. The next time someone asks for `example.com`, it responds instantly without re-querying the internet.  

✅ **Example: Checking cached DNS records**  
```sh
dig example.com
```

📌 If the result is already stored, the response time is much faster!


### ** 2. Forwarding Nameserver 📤**  
A **forwarding DNS server** does not resolve queries itself. Instead, it **forwards** DNS queries to another DNS server (usually a caching or authoritative server).  

🔹 Common in corporate networks to centralize DNS management.  

✅ **Example: Configuring a forwarding DNS server (Linux)**  
```sh
echo "nameserver 8.8.8.8" >> /etc/resolv.conf
```
This tells the system to **forward** all DNS queries to Google’s public DNS (`8.8.8.8`).

---

## ** 2️⃣  Authoritative Nameservers 🔐**  
Authoritative DNS servers store the **actual DNS records** for domains and provide direct answers to DNS queries.

### ** 1. Primary (Master) Nameserver 👑**  
- Holds the **original DNS records** for a domain.  
- Responsible for managing DNS records and updates.  

✅ **Example: Configuration for a primary nameserver (`named.conf`)**  
```sh
zone "example.com" {
    type master;
    file "/etc/bind/db.example.com";
};
```
📌 The file `/etc/bind/db.example.com` contains actual DNS records for `example.com`.


### **2. Secondary (Slave) Nameserver 📦**  
- Acts as a **backup** to the primary nameserver.  
- Copies data from the primary server but **does not** modify DNS records.  
- Improves reliability in case the primary server goes down.  

✅ **Example: Configuration for a secondary nameserver (`named.conf`)**  
```sh
zone "example.com" {
    type slave;
    file "/etc/bind/db.example.com";
    masters { 192.168.1.1; };
};
```
📌 The secondary nameserver syncs its data from the **primary server (192.168.1.1)**.

---

## **📊 Summary Table**  
| **DNS Type**                  | **Function** |
|-------------------------------|-------------|
| 🔄 **Recursive Nameserver**  | Finds DNS records by querying other servers |
| 🏪 **Caching Nameserver**    | Stores DNS responses temporarily to speed up queries |
| 📤 **Forwarding Nameserver** | Passes DNS queries to another server for resolution |
| 👑 **Primary Nameserver**    | Stores original DNS records for a domain |
| 📦 **Secondary Nameserver**  | Acts as a backup and gets data from the primary |

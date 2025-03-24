# **🌍 Forwarder Nameserver Setup Guide**  

## **📌 Introduction**  
A **Forwarder Nameserver** is a DNS server that forwards queries to an upstream DNS server instead of resolving them itself. This setup helps reduce query resolution time and improves efficiency by caching results.  

---

## **⚙️ Configure BIND for Forwarding**  
Edit the main configuration file:  
```bash
vim /etc/named.conf
```

### **🔧 Configuration Data:**  
```
 recursion yes;
         forwarders {
                8.8.8.8;
                8.8.4.4;
        };
```

### **📜 Main Configuration File (`named.conf`):**  
```
 //
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

acl ns_ip_add {
    127.0.0.1;
    192.168.112.145;
    192.168.112.146;
    192.168.112.147;
};

acl mynetwork {
    127.0.0.1;
    192.168.112.0/24;
};

options {
        listen-on port 53 { ns_ip_add; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { mynetwork; };

        /*
         - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
         - If you are building a RECURSIVE (caching) DNS server, you need to enable
           recursion.
         - If your recursive DNS server has a public IP address, you MUST enable access
           control to limit queries to your legitimate users. Failing to do so will
           cause your server to become part of large scale DNS amplification
           attacks. Implementing BCP38 within your network would greatly
           reduce such attack surface
        */
        recursion yes;

         forwarders {
                8.8.8.8;
                8.8.4.4;
        };
        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
        type hint;
        file "named.ca";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```

---

## **🔄 Restart and Enable Services**  
### **🚀 Restart BIND Service**  
```bash
systemctl restart named.service
```

### **🔥 Restart Firewall**  
```bash
systemctl restart firewalld.service
```

---

## **🛠️ Testing the Setup**  
### **🔎 Use `dig` to Test DNS Resolution**  
```bash
dig google.com @192.168.112.145
```
```bash
dig youtube.com @192.168.112.145
```

### **📡 Capture DNS Traffic Using `tcpdump`**  
```bash
tcpdump -t udp
```
```bash
tcpdump -i enp0s3 port 53 
```
```bash
tcpdump -n udp port 53
```
```bash
tcpdump -t udp -i enp0s3 port 53 
```
```bash
tcpdump -n -t udp -i enp0s3 port 53
```

---

## **📖 Explanation:**  
- **`forwarders`**: Specifies the external DNS servers to forward queries.  
- **`forward only`**: Forces the server to forward all queries instead of resolving them.  
- **`allow-query { any; };`**: Allows all clients to query the DNS server.  

---

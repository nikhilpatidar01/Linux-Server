
# Multiple Zone Configuration
# 1. Setup for patidar.local


## 📝 **Editing the DNS Zone Configuration**  
```bash
nano /etc/named.rfc1912.zones
```
### **Configuration Data:**
```
zone "patidar.local" IN {
        type master;
        file "forward.patidar.local";
        allow-update { none; };
};
```

## 📜 **Main DNS Zone File Configuration**  
```bash
// named.rfc1912.zones:
//
// Provided by Red Hat caching-nameserver package
//
// ISC BIND named zone configuration for zones recommended by
// RFC 1912 section 4.1 : localhost TLDs and address zones
// and https://tools.ietf.org/html/rfc6303
// (c)2007 R W Franks
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//
// Note: empty-zones-enable yes; option is default.
// If private ranges should be forwarded, add
// disable-empty-zone "."; into options
//

zone "localhost.localdomain" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "localhost" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "0.in-addr.arpa" IN {
        type master;
        file "named.empty";
        allow-update { none; };
};

zone "nikhil.local" IN {
        type master;
        file "forward.nikhil.local";
        allow-update { none; };
};

zone "112.168.192.in-addr.arpa" IN {
        type master;
        file "reverse.nikhil.local";
        allow-update { none; };
};

zone "patidar.local" IN {
        type master;
        file "forward.patidar.local";
        allow-update { none; };
};
```

## 📁 **Creating the Forward Zone File**  
### **📄 Copy Example Zone File and Modify:**
```bash
cd /var/named/
cp -v forward.nikhil.local forward.patidar.local
```

## ✏️ **Editing the Forward Zone File**  
```bash
nano /var/named/forward.patidar.local
```
## ✅ **Final Forward Zone Configuration**  
```bash
$TTL 1D
$ORIGIN patidar.local.
@      IN       SOA     ns1.nikhil.local. root.nikhil.local (
                                        20250324        ; serial
                                        3600            ; refresh
                                        1800            ; retry
                                        604800          ; expire
                                        86400 )         ; minimum
@               IN              NS      ns1.nikhil.local.
patidar.local.  IN              A       192.168.112.145
www             IN              CNAME   patidar.local.
router          IN              A       192.168.112.98
emp1            IN              A       192.168.112.200
emp2            IN              A       192.168.112.201

; Mail exchange
@               IN              MX      10 mail.nikhil.local.

; Text record for SPE
@               IN              TXT     "v=spr1 mx a ~all"

; Additional Services
ftp             IN              CNAME   patidar.local.
dev             IN              A       192.168.112.100
fileserver      IN              A       192.168.112.105
db              IN              A       192.168.112.110
test            IN              A       192.168.112.115
vpn             IN              A       192.168.112.120
git             IN              A       192.168.112.125
webapp          IN              A       192.168.112.130
logs            IN              A       192.168.112.135
```

## 🔐 **Setting Permissions**  
```bash
chgrp named /var/named/forward.patidar.local
chmod 640 /var/named/forward.patidar.local
```

## 🔄 **Restarting BIND Service**  
```bash
systemctl restart named
```

## 🔍 **Checking DNS Resolution**  
```bash
dig patidar.local
```

## ✅ **Validating Configuration**  
```bash
named-checkconf
named-checkconf /etc/named.conf
named-checkconf /etc/named.rfc1912.zones
named-checkconf /etc/named.root.key
```

## 🔎 **Verifying Forward Zone**  
```bash
named-checkzone patidar.local /var/named/forward.patidar.local
```
### **Output:**
```
zone patidar.local/IN: loaded serial 20250324
OK
```

## 🔬 **Testing Name Resolution**  
```bash
dig mx patidar.local
dig txt patidar.local
dig a patidar.local
```

---

# 2. Setup for ai.local


## 📝 **Editing the DNS Zone Configuration**  
```bash
nano /etc/named.rfc1912.zones
```
### **Configuration Data:**
```
zone "ai.local" IN {
        type master;
        file "forward.ai.local";
        allow-update { none; };
};
```

## 📜 **Main DNS Zone File Configuration**  
```bash
// named.rfc1912.zones:
//
// Provided by Red Hat caching-nameserver package
//
// ISC BIND named zone configuration for zones recommended by
// RFC 1912 section 4.1 : localhost TLDs and address zones
// and https://tools.ietf.org/html/rfc6303
// (c)2007 R W Franks
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//
// Note: empty-zones-enable yes; option is default.
// If private ranges should be forwarded, add
// disable-empty-zone "."; into options
//

zone "localhost.localdomain" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "localhost" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "0.in-addr.arpa" IN {
        type master;
        file "named.empty";
        allow-update { none; };
};

zone "nikhil.local" IN {
        type master;
        file "forward.nikhil.local";
        allow-update { none; };
};

zone "112.168.192.in-addr.arpa" IN {
        type master;
        file "reverse.nikhil.local";
        allow-update { none; };
};

zone "patidar.local" IN {
        type master;
        file "forward.patidar.local";
        allow-update { none; };
};

zone "ai.local" IN {
        type master;
        file "forward.ai.local";
        allow-update { none; };
};
```

## 📁 **Creating the Forward Zone File**  
### **📄 Copy Example Zone File and Modify:**
```bash
cd /var/named/
cp -v forward.patidar.local forward.ai.local
```

## ✏️ **Editing the Forward Zone File**  
```bash
nano /var/named/forward.ai.local
```
## ✅ **Final Forward Zone Configuration**  
```bash
$TTL 1D
$ORIGIN ai.local.
@      IN       SOA     ns1.nikhil.local. root.nikhil.local (
                                        20250324        ; serial
                                        3600            ; refresh
                                        1800            ; retry
                                        604800          ; expire
                                        86400 )         ; minimum
@               IN              NS      ns1.nikhil.local.
ai.local.       IN              A       192.168.112.145
www             IN              CNAME   ai.local.
router          IN              A       192.168.112.98
emp1            IN              A       192.168.112.200
emp2            IN              A       192.168.112.201

; Mail exchange
@               IN              MX      10 mail.nikhil.local.

; Text record for SPE
@               IN              TXT     "v=spr1 mx a ~all"

; Additional Services
ftp             IN              CNAME   ai.local.
dev             IN              A       192.168.112.100
fileserver      IN              A       192.168.112.105
db              IN              A       192.168.112.110
test            IN              A       192.168.112.115
vpn             IN              A       192.168.112.120
git             IN              A       192.168.112.125
webapp          IN              A       192.168.112.130
logs            IN              A       192.168.112.135
```

## 🔐 **Setting Permissions**  
```bash
chgrp named /var/named/forward.ai.local
chmod 640 /var/named/forward.ai.local
```

## 🔄 **Restarting BIND Service**  
```bash
systemctl restart named
```

## 🔍 **Checking DNS Resolution**  
```bash
dig ai.local
```

## ✅ **Validating Configuration**  
```bash
named-checkconf
named-checkconf /etc/named.conf
named-checkconf /etc/named.rfc1912.zones
named-checkconf /etc/named.root.key
```

## 🔎 **Verifying Forward Zone**  
```bash
named-checkzone ai.local /var/named/forward.ai.local
```
### **Output:**
```
zone ai.local/IN: loaded serial 20250324
OK
```

## 🔬 **Testing Name Resolution**  
```bash
dig mx ai.local
dig txt ai.local
dig a ai.local
```

---

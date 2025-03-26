# Master and Slave DNS Server

## Server Details

| Role        | IP Address        | Description         |
|------------|------------------|---------------------|
| Master DNS | 192.168.112.145  | Primary DNS Server |
| Slave DNS  | 192.168.112.160  | Secondary DNS Server |
| Domain Name| nikhil.local     | Example Domain     |


# Master DNS Server Configuration

## 1. Set the Hostname
```
hostnamectl set-hostname ns1.nikhil.local
```
### Check
```
hostname
```

## 2. Install BIND and Utilities
```
yum install bind bind-utils -y
```

## 3. Configure the DNS Zones
```
nano /etc/named.rfc1912.zones
```

### Configuration Zone
```
zone "nikhil.local" IN {
        type master;
        file "forward.nikhil.local";
        allow-update { none; };
        allow-transfer { 192.168.1.160; };        
};

zone "patidar.local" IN {
        type master;
        file "forward.patidar.local";
        allow-update { none; };
        allow-transfer { 192.168.1.160; };
};

zone "ai.local" IN {
        type master;
        file "forward.ai.local";
        allow-update { none; };
        allow-transfer { 192.168.1.160; };
};
```

## 4. Create the Zone file
### 4.1 Forward Zone for nikhil.local
```
nano /var/named/forward.nikhil.local
```
```
$TTL 1D
@       IN SOA  ns1.nikhil.local. root.nikhil.local (
                                        20250324        ; serial
                                        3600            ; refresh
                                        1800            ; retry
                                        604800          ; expire
                                        86400 )         ; minimum
@               IN              NS      ns1.nikhil.local.
@               IN              NS      ns2.nikhil.local.

ns1             IN              A       192.168.112.145
ns2             IN              A       192.168.112.160
nikhil.local.   IN              A       192.168.112.145
www             IN              CNAME   nikhil.local.
```

## 5. Configure Firewall Rules (Firewalld)

```
firewall-cmd --permanent --add-port=53/udp
```
```
firewall-cmd --permanent --add-port=53/tcp
```
```
firewall-cmd --reload
```

## 6. Validate Configuration Check
```
named-checkconf
```
```
named-checkconf /etc/named.conf
```
```
named-checkconf /etc/named.rfc1912.zones
```

## 7. Verifying Forward Zone  
### Create then Verify:
```
named-checkzone nikhil.local /var/named/forward.nikhil.local
```
### **Output:**
```
zone nikhil.local/IN: loaded serial 20250324
OK
```

## 8. Start and Enable BIND
```
systemctl restart named.service
```
```
systemctl enable named.service
```

---

# 🖥️ **Secondary DNS Server (Slave) Configuration**  

## **1. Install and Configure BIND**
```
rpm -qa | grep bind
```
```
yum install bind-utils -y
```
```
rpm -qa | grep bind
```

## **2. Set the Hostname for the DNS Server**  
```
nano /etc/hostname
```
### Example:
```
ns2.nikhil.local
```

## **3. Editing the Host File as per the Zone**
```
nano /etc/hosts
```
### **Configuration Data (Optional):**
```bash
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost.localdomain6
192.168.112.160 ns2 ns2.nikhil.local
```

## **4. Configure the Zones on the Slave**
```
nano /etc/named.rfc1912.zones
```
```
zone "nikhil.local" IN {
        type slave;
        masters { 192.168.112.145; };
        file "slaves/forward.nikhil.local";
};

zone "patidar.local" IN {
        type slave;
        masters { 192.168.112.145; };
        file "slaves/forward.patidar.local";
};

zone "ai.local" IN {
        type slave;
        masters { 192.168.112.145; };
        file "slaves/forward.ai.local" ;
};```
```
## **9. Main File Configuration**
### Add ip 
```
nano /etc/named.conf
```
```bash
options {
        listen-on port 53 { 127.0.0.1; 192.168.112.160; };    
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; 192.168.112.160; };
```

## **5. Configure Firewall Rules (Firewalld)**

```
firewall-cmd --permanent --add-port=53/udp
```
```
firewall-cmd --permanent --add-port=53/tcp
```
```
firewall-cmd --reload
```

## **6. Validate Configuration Check**
```
named-checkconf
```
```
named-checkconf /etc/named.conf
```

## **7. Verifying Forward Zone**  
```
named-checkzone nikhil.local /var/named/forward.nikhil.local
```
### **Output:**
```
zone nikhil.local/IN: loaded serial 20250324
OK
```

## **8. Start and Enable BIND**
```
systemctl restart named.service
```
```
systemctl enable named.service
```

## **9. Test Setup**
### **Use `dig` to Test DNS Resolution**
```
dig nikhil.local @192.168.112.160
```
```
dig patidar.local @192.168.112.160
```


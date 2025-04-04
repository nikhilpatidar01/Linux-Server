
# **Reverse Zone Configuration**  

## **Edit DNS Zone Configuration:**
```bash
nano /etc/named.rfc1912.zones
```

## **Configuration Data:**
```
zone "112.168.192.in-addr.arpa" IN {
        type master;
        file "reverse.nikhil.local";
        allow-update { none; };
};
```

## **Main File Configuration:**
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
```

## **Create Forward Zone File**  
### **Copy the Example Zone File and Modify it:**
```bash
cd /var/named/
cp -v /var/named/forward.nikhil.local /var/named/reverse.nikhil.local
```

## **Create Reverse Zone File**
```bash
nano /var/named/reverse.nikhil.local
```

## **Configure File:**
```
$TTL 1D
@       IN SOA  ns1.nikhil.local. root.nikhil.local (
                                        20250324        ; serial
                                        3600            ; refresh
                                        1800            ; retry
                                        604800          ; expire
                                        86400 )         ; minimum
@               IN              NS      ns1.nikhil.local.
@               IN              PTR     ns1.nikhil.local.
145             IN              PTR     ns1.nikhil.local.
200             IN              PTR     emp1.nikhil.local
201             IN              PTR     emp2.nikhil.local.
205             IN              PTR     test.nikhil.local.
210             IN              PTR     mail.nikhil.local.
215             IN              PTR     db.nikhil.local.
220             IN              PTR     vpn.nikhil.local.
145             IN              PTR     www.nikhil.local.
```

## **Set Permission:**
```bash
chgrp named /var/named/reverse.nikhil.local
chmod 640 /var/named/reverse.nikhil.local
```

## **Restart BIND Service:**
```bash
systemctl restart named
```

## **Check Configuration:**
```bash
named-checkconf
named-checkconf /etc/named.conf
named-checkconf /etc/named.rfc1912.zones
named-checkconf /etc/named.root.key
```

## **Verify Forward Zone:**
```bash
named-checkzone 112.168.192.in-addr.arpa /var/named/reverse.nikhil.local
```

### **Output:**
```
zone 112.168.192.in-addr.arpa/IN: loaded serial 20250324
OK
```

## **Test Resolution:**
```bash
dig -x 192.168.112.145
dig -x 192.168.112.200
dig -x 192.168.112.210
```

---

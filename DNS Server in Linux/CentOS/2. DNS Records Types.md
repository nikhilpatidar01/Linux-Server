# 🌐 **DNS Records **  

DNS records define how **domain names** are mapped to **IP addresses** and other resources. These records are stored in **authoritative DNS servers** and help direct internet traffic correctly.  

---

## **📌 Common DNS Record Types & Commands**  

### **1️⃣ A (Address) Record** 🏠  
🔹 Maps a **domain name** to an **IPv4 address**.  

✅ **Example:**  
```
example.com. IN A 192.168.1.1
```
✅ **Linux Command:**  
```sh
dig example.com A
```

---

### **2️⃣ AAAA (IPv6 Address) Record** 🌍  
🔹 Maps a **domain name** to an **IPv6 address**.  

✅ **Example:**  
```
example.com. IN AAAA 2001:0db8::1
```
✅ **Linux Command:**  
```sh
dig example.com AAAA
```

---

### **3️⃣ CNAME (Canonical Name) Record** 🔄  
🔹 Creates an **alias** for another domain.  

✅ **Example:**  
```
blog.example.com. IN CNAME example.com.
```
✅ **Linux Command:**  
```sh
dig blog.example.com CNAME
```

---

### **4️⃣ NS (Name Server) Record** 🏢  
🔹 Specifies the **authoritative name servers** for a domain.  

✅ **Example:**  
```
example.com. IN NS ns1.example.com.
example.com. IN NS ns2.example.com.
```
✅ **Linux Command:**  
```sh
dig example.com NS
```

---

### **5️⃣ PTR (Pointer) Record** 🔄  
🔹 Used for **reverse DNS lookups** (IP to domain name).  

✅ **Example:**  
```
1.1.168.192.in-addr.arpa. IN PTR example.com.
```
✅ **Linux Command:**  
```sh
dig -x 192.168.1.1
```

---

### **6️⃣ SOA (Start of Authority) Record** 📜  
🔹 Provides **administrative information** about the domain.  

✅ **Example:**  
```
example.com. IN SOA ns1.example.com. admin.example.com. (
  2025031101 ; Serial
  3600       ; Refresh
  1800       ; Retry
  1209600    ; Expire
  86400      ; Minimum TTL
)
```
✅ **Linux Command:**  
```sh
dig example.com SOA
```

---

### **7️⃣ HINFO (Host Information) Record** 💻  
🔹 Provides **hardware & OS details** for a host.  

✅ **Example:**  
```
example.com. IN HINFO "Intel i7" "Linux"
```
✅ **Linux Command:**  
```sh
dig example.com HINFO
```

---

### **8️⃣ MX (Mail Exchanger) Record** 📧  
🔹 Specifies **email servers** for a domain.  

✅ **Example:**  
```
example.com. IN MX 10 mail.example.com.
```
✅ **Linux Command:**  
```sh
dig example.com MX
```

---

### **9️⃣ TXT (Text) Record** 📝  
🔹 Stores arbitrary text, often used for **SPF, DKIM, and verification records**.  

✅ **Example (SPF Record):**  
```
example.com. IN TXT "v=spf1 include:_spf.google.com ~all"
```
✅ **Linux Command:**  
```sh
dig example.com TXT
```

---

## **📊 Summary Table of DNS Records**  

| **DNS Record** | **Purpose** | **Example Command** |
|--------------|-----------|-----------------|
| **A** | Maps domain to IPv4 | `dig example.com A` |
| **AAAA** | Maps domain to IPv6 | `dig example.com AAAA` |
| **CNAME** | Alias for another domain | `dig blog.example.com CNAME` |
| **NS** | Specifies name servers | `dig example.com NS` |
| **PTR** | Reverse lookup (IP → Domain) | `dig -x 192.168.1.1` |
| **SOA** | Admin info for a domain | `dig example.com SOA` |
| **HINFO** | Host hardware & OS details | `dig example.com HINFO` |
| **MX** | Mail server info | `dig example.com MX` |
| **TXT** | Stores text data | `dig example.com TXT` |

---

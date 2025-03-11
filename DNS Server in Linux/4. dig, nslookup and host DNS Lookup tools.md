
# **🛠️ `dig` Command – DNS Lookup & Troubleshooting Tool**  


## **🌍 Basic `dig` Commands**  

### ✅ **Perform a Basic DNS Lookup**  
Simply running `dig` opens an **interactive mode**:  
```sh
dig
```

### ✅ **Lookup a Specific Domain**  
Query DNS records for a domain:  
```sh
dig google.com
```

### ✅ **Query a Specific DNS Server**  
Use a **custom DNS server** instead of the default resolver:  
```sh
dig google.com @64.6.64.6
```
🔹 This queries `google.com` using the **DNS server 64.6.64.6**.

---

## **📌 Query Specific DNS Record Types**  

### 🔹 **A Record (IPv4 Address Lookup)**  
```sh
dig google.com A
dig A google.com
```

### 🔹 **AAAA Record (IPv6 Address Lookup)**  
```sh
dig google.com AAAA
dig AAAA google.com
```

### 🔹 **NS Record (Name Server Lookup)**  
```sh
dig google.com NS
dig NS google.com
```

### 🔹 **MX Record (Mail Server Lookup)**  
```sh
dig google.com MX
```

### 🔹 **CNAME Record (Alias Lookup)**  
```sh
dig google.com CNAME
```

### 🔹 **TXT Record (SPF, DKIM, etc.)**  
```sh
dig google.com TXT
```

### 🔹 **SOA Record (Start of Authority)**  
```sh
dig google.com SOA
```

### 🔹 **ANY Record (All Available DNS Info)**  
```sh
dig google.com ANY
```

---

## **📌 📉 Shortened Output for Clean Results**  

### ✅ **Short Answer Format**  
Remove unnecessary details and show only essential info:  
```sh
dig google.com +short
```

### ✅ **Get A Record in Short Format**  
```sh
dig google.com A +short
```

### ✅ **Get AAAA Record in Short Format**  
```sh
dig google.com AAAA +short
```

### ✅ **Get NS Record in Short Format**  
```sh
dig google.com NS +short
```

### ✅ **Get MX Record in Short Format**  
```sh
dig google.com MX +short
```

### ✅ **Get ANY Record in Short Format**  
```sh
dig google.com ANY +short
```

---

## **📌 Customizing Output Display**  

### ✅ **Suppress All Output Except Answer**  
```sh
dig google.com +noall +answer
```

### ✅ **Display Only Question & Answer Sections**  
```sh
dig google.com +noall +answer +question
```

### ✅ **Fine-tune Output (Suppress Extra Details)**  
```sh
dig google.com +nocomments +noquestion +noauthority +noadditional +nostats
```

---

## **📌 Bulk DNS Lookups (Multiple Domains at Once)**  

### ✅ **Create a File with Domains**  
```sh
vim domain_list.txt
```
**Example file content:**
```
google.com
facebook.com
armourinfosec.com
youtube.com
```

### ✅ **Query Multiple Domains from a File**  
```sh
dig -f domain_list.txt
```

### ✅ **Get Shortened Output for Multiple Domains**  
```sh
dig -f domain_list.txt +short
```

### ✅ **Save Output to a File**  
```sh
dig -f domain_list.txt +short > ip_addresses.txt
```

---

## **📌 🔄 Reverse DNS Lookup (PTR Records)**  

### ✅ **Find Hostname from an IP Address**  
```sh
dig -x 8.8.8.8
```

### ✅ **Shortened Reverse Lookup Output**  
```sh
dig -x 8.8.8.8 +short
```

---

## **📌 Bulk Reverse DNS Lookups**  

### ✅ **Create a File with IP Addresses**  
```sh
vim ip_list.txt
```
**Example file content:**
```
64.6.64.6
8.8.4.4
1.1.1.1
20.20.20.20
```

### ✅ **Convert IPs to `-x` Format (for Reverse Lookup)**  

Using `awk`:  
```sh
awk '{print "-x " $0}' ip_list.txt > ip_list2.txt
```

Using `sed`:  
```sh
cat ip_list.txt | sed -e "s/.*/-x &/" > ip_list2.txt
```

### ✅ **Perform Reverse Lookup for Multiple IPs**  
```sh
dig -f ip_list2.txt +short
```

---

## **📊 Summary Table of `dig` Commands**  

| **Query Type** | **Command** |
|--------------|------------|
| Basic Domain Lookup | `dig google.com` |
| Query Specific DNS Server | `dig google.com @8.8.8.8` |
| A Record (IPv4) | `dig google.com A` |
| AAAA Record (IPv6) | `dig google.com AAAA` |
| MX Record (Mail Server) | `dig google.com MX` |
| NS Record (Name Server) | `dig google.com NS` |
| CNAME Record (Alias) | `dig google.com CNAME` |
| TXT Record (SPF, DKIM, etc.) | `dig google.com TXT` |
| SOA Record (Start of Authority) | `dig google.com SOA` |
| PTR Record (Reverse Lookup) | `dig -x 8.8.8.8` |
| Short Output (Only IP) | `dig google.com +short` |
| Recursive Query | `dig google.com +trace` |
| Query Multiple Domains | `dig -f domain_list.txt` |
| Query Multiple IPs (Reverse Lookup) | `dig -f ip_list.txt +short` |


---

# **🛠️ `nslookup` Command – DNS Lookup & Troubleshooting Tool**  

---

## **🌍 Basic `nslookup` Commands**  

### ✅ **Basic Domain Lookup**  
To get the **IP address** of a domain:  
```sh
nslookup google.com
```

### ✅ **Interactive Mode**  
Run `nslookup` without arguments to enter interactive mode:  
```sh
nslookup
```
Once inside, type:  
```sh
> google.com
> exit
```

---

## **📌 Query a Specific DNS Server**  

### ✅ **Use a Custom DNS Server**  
Query a specific DNS server instead of the default:  
```sh
nslookup google.com 8.8.8.8
```
🔹 This queries `google.com` using Google’s **8.8.8.8 DNS server**.

---

## **📌 Query Specific DNS Record Types**  

### 🔹 **A Record (IPv4 Address Lookup)**  
```sh
nslookup -type=A google.com
```

### 🔹 **AAAA Record (IPv6 Address Lookup)**  
```sh
nslookup -type=AAAA google.com
```

### 🔹 **NS Record (Name Server Lookup)**  
```sh
nslookup -type=NS google.com
```

### 🔹 **MX Record (Mail Server Lookup)**  
```sh
nslookup -type=MX google.com
```

### 🔹 **CNAME Record (Alias Lookup)**  
```sh
nslookup -type=CNAME google.com
```

### 🔹 **TXT Record (SPF, DKIM, etc.)**  
```sh
nslookup -type=TXT google.com
```

### 🔹 **SOA Record (Start of Authority)**  
```sh
nslookup -type=SOA google.com
```

---

## **📌 Reverse DNS Lookup (PTR Record)**  

### ✅ **Find Hostname from an IP Address**  
```sh
nslookup 8.8.8.8
```
🔹 This returns the **hostname** associated with **8.8.8.8**.

---

## **📌 Bulk DNS Lookups (Multiple Domains at Once)**  

### ✅ **Create a File with Domains**  
```sh
vim domain_list.txt
```
**Example file content:**
```
google.com
facebook.com
armourinfosec.com
youtube.com
```

### ✅ **Query Multiple Domains Using a Loop**  
```sh
while read domain; do nslookup -type=A $domain; done < domain_list.txt
```

---

## **📌 Configure nslookup Settings**  

### ✅ **Set a Specific DNS Server for Multiple Queries**  
Enter **interactive mode**:  
```sh
nslookup
```
Then set a specific DNS server:  
```sh
> server 8.8.8.8
```
Now, all queries will use **8.8.8.8** until you exit.

---

## **📊 Summary Table of `nslookup` Commands**  

| **Query Type** | **Command** |
|--------------|------------|
| Basic Domain Lookup | `nslookup google.com` |
| Query Specific DNS Server | `nslookup google.com 8.8.8.8` |
| A Record (IPv4) | `nslookup -type=A google.com` |
| AAAA Record (IPv6) | `nslookup -type=AAAA google.com` |
| MX Record (Mail Server) | `nslookup -type=MX google.com` |
| NS Record (Name Server) | `nslookup -type=NS google.com` |
| CNAME Record (Alias) | `nslookup -type=CNAME google.com` |
| TXT Record (SPF, DKIM, etc.) | `nslookup -type=TXT google.com` |
| SOA Record (Start of Authority) | `nslookup -type=SOA google.com` |
| PTR Record (Reverse Lookup) | `nslookup 8.8.8.8` |
| Set Custom DNS Server | `nslookup > server 8.8.8.8` |

---

# **🛠️ `host` Command – DNS Lookup & Troubleshooting Tool**  

---

## **🌍 Basic `host` Commands**  

### ✅ **Basic Domain Lookup**  
Find the **IP address** of a domain:  
```sh
host google.com
```

### ✅ **Query a Specific DNS Server**  
Instead of using the default DNS server, specify a custom one:  
```sh
host google.com 8.8.8.8
```
🔹 This queries `google.com` using **Google’s 8.8.8.8 DNS server**.

---

## **📌 Query Specific DNS Record Types**  

### 🔹 **A Record (IPv4 Address Lookup)**  
```sh
host -t A google.com
```

### 🔹 **AAAA Record (IPv6 Address Lookup)**  
```sh
host -t AAAA google.com
```

### 🔹 **NS Record (Name Server Lookup)**  
```sh
host -t NS google.com
```

### 🔹 **MX Record (Mail Server Lookup)**  
```sh
host -t MX google.com
```

### 🔹 **CNAME Record (Alias Lookup)**  
```sh
host -t CNAME google.com
```

### 🔹 **TXT Record (SPF, DKIM, etc.)**  
```sh
host -t TXT google.com
```

### 🔹 **SOA Record (Start of Authority)**  
```sh
host -t SOA google.com
```

---

## **📌 Reverse DNS Lookup (PTR Record)**  

### ✅ **Find Hostname from an IP Address**  
```sh
host 8.8.8.8
```
🔹 This returns the **hostname** associated with **8.8.8.8**.

---

## **📌 Bulk DNS Lookups (Multiple Domains at Once)**  

### ✅ **Create a File with Domains**  
```sh
vim domain_list.txt
```
**Example file content:**
```
google.com
facebook.com
armourinfosec.com
youtube.com
```

### ✅ **Query Multiple Domains Using a Loop**  
```sh
while read domain; do host $domain; done < domain_list.txt
```

---

## **📊 Summary Table of `host` Commands**  

| **Query Type** | **Command** |
|--------------|------------|
| Basic Domain Lookup | `host google.com` |
| Query Specific DNS Server | `host google.com 8.8.8.8` |
| A Record (IPv4) | `host -t A google.com` |
| AAAA Record (IPv6) | `host -t AAAA google.com` |
| MX Record (Mail Server) | `host -t MX google.com` |
| NS Record (Name Server) | `host -t NS google.com` |
| CNAME Record (Alias) | `host -t CNAME google.com` |
| TXT Record (SPF, DKIM, etc.) | `host -t TXT google.com` |
| SOA Record (Start of Authority) | `host -t SOA google.com` |
| PTR Record (Reverse Lookup) | `host 8.8.8.8` |

---

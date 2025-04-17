
# **TFTP (Trivial File Transfer Protocol) Guide**

---

## **What is TFTP?**

TFTP is a lightweight file transfer protocol used to send and receive files between systems over a network. It’s commonly used in environments where simplicity is key and advanced features are unnecessary.

---

## **How TFTP Works**

- **Protocol:** Uses **UDP** (User Datagram Protocol) for data transfer.
- **Server & Client:** A TFTP server hosts files; clients can send or retrieve them.
- **No Authentication:** No login required—any device with access to the network can use it.

---

## **Common Uses of TFTP**

- Booting diskless systems, routers, or embedded devices.
- Uploading firmware or configuration files.
- Downloading log files or backup configs.
- Lightweight network file transfers in controlled environments.

---

## **Limitations of TFTP**

- ❌ **No Security:** No encryption or authentication mechanisms.
- ❌ **No Error Checking:** Relies on the application to verify data integrity.
- ❌ **Minimal File Management:** No ability to rename, delete, or list files on the server.

---

## **Related Protocols**

- **FTP:** TCP-based protocol for secure file transfer with user authentication.
- **BOOTP:** Often works with TFTP to help network devices boot and get IP configurations.

---

## **Setting Up a TFTP Server on CentOS**

### **1. Install TFTP and TFTP Server**
```bash
yum install tftp tftp-server -y
```

### **2. Check the TFTP Server Package**
```bash
rpm -qi tftp-server
```

### **3. Configure the TFTP Service**

- Default TFTP root directory: `/var/lib/tftpboot/`
- The TFTP service is not enabled by default.

#### **Copy Systemd Service Files**
```bash
cp -v /usr/lib/systemd/system/tftp.service /etc/systemd/system/tftp-server.service
cp -v /usr/lib/systemd/system/tftp.socket /etc/systemd/system/tftp-server.socket
```

#### **Check the TFTP Manual**
```bash
man in.tftpd
```

#### **Edit Service Configuration**
```bash
vim /etc/systemd/system/tftp-server.service
vim /etc/systemd/system/tftp-server.socket
```
- Ensure the socket is set to listen on **UDP port 69**.

---

### **4. Set Up the TFTP Root Directory**
```bash
cd /var/lib/tftpboot
chmod -R 777 /var/lib/tftpboot
```
- Place all files to be served in this directory.

---

### **5. Enable and Start the TFTP Service**
```bash
systemctl daemon-reexec
systemctl restart tftp-server.socket
systemctl enable tftp-server.socket
```

---

### **6. Open Port 69 in the Firewall**
```bash
iptables -A INPUT -p udp --dport 69 -j ACCEPT
service iptables restart
```

---

## **Using TFTP as a Client**

1. **Connect to the TFTP Server**
```bash
tftp 192.168.1.31
```

2. **Download a File**
```bash
get filename
```

3. **Upload a File**
```bash
put localfile remotefile
```

> ⚠️ **Note:** TFTP requires that the filename be known beforehand, as it doesn’t support directory browsing.

---

## **Testing from a Windows PC**

- Install a TFTP client (e.g., **TFTPD32**, **TFTPD64**, or enable the built-in TFTP client in Windows).
- Use similar `get` and `put` commands to test upload/download.

---

## **Conclusion**

- ✅ **TFTP is fast and lightweight**, suitable for trusted networks and simple use cases.
- ❌ **Not secure**—avoid using it on public or untrusted networks.
- 📦 For sensitive or authenticated transfers, use **FTP**, **SFTP**, or **SCP**.

---

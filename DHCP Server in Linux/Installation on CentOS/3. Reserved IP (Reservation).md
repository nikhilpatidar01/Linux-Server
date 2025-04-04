
# **🔒 Reserving an IP Address in DHCP**  

You can **reserve specific IP addresses** for particular clients based on their **MAC addresses** in the DHCP configuration file. This ensures that the device **always receives the same IP address**, which is useful for **servers, printers, and critical devices**.  

---

## **1️⃣ Edit DHCP Configuration File**  

📝 Open the DHCP configuration file for editing:  
```bash
vim /etc/dhcp/dhcpd.conf
```

---

## **2️⃣ Example Configurations for IP Reservation**  

To **reserve an IP address**, add a `host` block inside the **subnet configuration** with the **MAC address and fixed IP**.  

### **Example 1: Reserve an IP Address for a Single Client**  

This example reserves **IP 192.168.1.201** for a client with **MAC address 08:00:27:ab:46:cf**.  

```bash
authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {
    
    # Specify the range of lease IP addresses
    range 192.168.1.50 192.168.1.50;
    range 192.168.1.61 192.168.1.210;
    range 192.168.1.231 192.168.1.250;

    # Default gateway
    option routers 192.168.1.1;

    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Broadcast address
    option broadcast-address 192.168.1.255;

    # Default lease time (in seconds)
    default-lease-time 600;

    # Maximum lease time (in seconds)
    max-lease-time 7200;
}

# Reserve IP for a specific device
host win11-1 {
    hardware ethernet 08:00:27:ab:46:cf;
    fixed-address 192.168.1.201;
}
```

---

### **Example 2: Reserve Multiple IP Addresses**  

This example reserves **two IP addresses** for different clients.  

```bash
# Reserve IP for win11-1
host win11-1 {
    hardware ethernet 08:00:27:ab:46:cf;
    fixed-address 192.168.1.201;
}

# Reserve IP for win11-2
host win11-2 {
    hardware ethernet 08:00:27:d6:86:94;
    fixed-address 192.168.1.150;
}
```

---

## **3️⃣ Restart DHCP Service**  

After modifying the configuration file, restart the DHCP service for changes to take effect:  

```bash
systemctl restart dhcpd
```

---

## **🛠️ Explanation**  

✅ `hardware ethernet` → The **MAC address** of the client device.  
✅ `fixed-address` → The **static IP address** to assign to the specified MAC address.  
✅ **Effect**: When a client with a **matching MAC address** requests an IP, the DHCP server will assign the reserved IP.  

### **⚠️ Important Considerations**  
- **Reserved IP addresses should be outside the DHCP pool range** to avoid conflicts.  
- Ensure that the **reserved IP addresses are not part of the dynamic IP range**.  
- If a reserved IP address is needed **before lease expiration**, restart the DHCP client on the device or release the current lease.  

---

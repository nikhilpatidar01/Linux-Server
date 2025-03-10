

# **🔐 Allow List (Whitelist) and Deny List (Blacklist) in DHCP**  

You can configure a DHCP server to **only provide IP addresses to specific known clients** by using the `deny unknown-clients` directive. This ensures that **only devices with specified MAC addresses can receive an IP address** from the DHCP server.  

---

## **1️⃣ Edit the DHCP Configuration File**  

📝 Open the DHCP configuration file for editing:  
```bash
vim /etc/dhcp/dhcpd.conf
```

---

## **2️⃣ Example Configuration for Whitelisting Clients**  

To allow only **specific MAC addresses** to receive an IP address, add `deny unknown-clients;` inside the subnet block and define `host` entries for the allowed clients.  

### **Example: Allow Only Specific Clients**  

This example **only allows devices with MAC addresses** `08:00:27:d6:86:94` and `08:00:27:fd:12:34` to receive an IP.  

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

    # Deny unknown clients from getting an IP address
    deny unknown-clients;
}

# Allow only specific devices
host device1 {
    hardware ethernet 08:00:27:d6:86:94;
    fixed-address 192.168.1.100;
}

host device2 {
    hardware ethernet 08:00:27:fd:12:34;
    fixed-address 192.168.1.101;
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
✅ `deny unknown-clients;` → Prevents **any device that is not explicitly listed** in the configuration from getting an IP address.  

### **⚠️ Benefits of Whitelisting**  
🔹 **Enhances network security** by restricting IP assignments to known devices.  
🔹 **Prevents unauthorized devices** from connecting to the network.  
🔹 **Reduces IP conflicts** by ensuring only approved devices get IPs.  

---

Here's a **corrected and formatted** guide for **blocking multiple clients (deny list) in DHCP**:  

---

# **⛔ Deny List (Blacklist) in DHCP**  

You can configure your **DHCP server to block specific devices** from receiving an IP address by using the `deny booting;` directive.  

---

## **1️⃣ Edit the DHCP Configuration File**  

📝 Open the DHCP configuration file for editing:  
```bash
vim /etc/dhcp/dhcpd.conf
```

---

## **2️⃣ Example Configuration for Blocking Clients**  

To **deny multiple MAC addresses** from receiving an IP, add separate `host` blocks with `deny booting;` inside each one.  

### **Example: Block Multiple Clients**  

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

# Block specific devices
host win11-1 {
    hardware ethernet 08:00:27:ab:46:cf;
    deny booting;
}

host win11-2 {
    hardware ethernet 08:00:27:d6:86:94;
    deny booting;
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
✅ `deny booting;` → **Prevents the client from receiving an IP address** from the DHCP server.  
✅ When a client **with a matching MAC address** requests an IP, **the DHCP server ignores the request**.  

### **⚠️ Benefits of Denying Clients**  
🔹 **Blocks unauthorized devices** from connecting to the network.  
🔹 **Prevents unwanted clients** from obtaining IP addresses.  
🔹 **Enhances security** by controlling DHCP assignments.  

---

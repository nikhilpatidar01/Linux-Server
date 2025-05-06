
## Debian 12 Setup Guide

This guide outlines the steps to set up a basic Debian 12 system: configuring networking, enabling SSH, and installing essential utilities like Zsh.

---

### 1. SSH into the Debian System

```bash
ssh root@192.168.1.25
```

---

### 2. Check System Architecture

```bash
dpkg --print-architecture
```

---

### 3. Update and Upgrade the System

```bash
apt update
apt upgrade -y
```

---

### 4. Install Basic Tools

```bash
apt install bash nano net-tools -y
```

---

### 5. Install SSH Server and Client

```bash
apt install openssh-server openssh-client ssh -y
```

Edit SSH configuration if needed:

```bash
nano /etc/ssh/sshd_config
```

**Add this line at the end of the file:**
   ```bash
   PermitRootLogin yes
   ```
Also ensure **PasswordAuthentication is enabled**. Uncomment and set:
   ```bash
   PasswordAuthentication yes
   ```



Enable and start SSH:

```bash
systemctl enable ssh
systemctl start ssh
systemctl restart ssh
```

---

### 6. Configure Network Interfaces

Edit the interfaces file:

```bash
nano /etc/network/interfaces
```

Example configuration:

```ini
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface (enp0s3)
allow-hotplug enp0s3
iface enp0s3 inet static
    address 192.168.1.25
    netmask 255.255.255.0
    network 192.168.1.0
    broadcast 192.168.1.255
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8
iface enp0s3 inet6 auto

# The secondary network interface (enp0s8)
allow-hotplug enp0s8
iface enp0s8 inet static
    address 192.168.2.10
    netmask 255.255.255.0
    network 192.168.2.0
    broadcast 192.168.2.255
    dns-nameservers 8.8.8.8
iface enp0s8 inet6 auto
```

---

### 7. Configure DNS Resolver

```bash
nano /etc/resolv.conf
```

Example content:

```
nameserver 8.8.8.8
```

---

### 8. Restart Network Services

```bash
systemctl restart networking
```

Bring interfaces down and up:

```bash
ifdown enp0s3 && ifup enp0s3
ifdown enp0s8 && ifup enp0s8
```

Reboot:

```bash
reboot
```

---

### 9. SSH from a Different User

```bash
ssh nikhil@192.168.1.21
```

Switch to root:

```bash
su - root
```

---

### 10. Configure APT Sources

Edit the source list:

```bash
nano /etc/apt/sources.list
```

Install HTTPS transport support:

```bash
apt install apt-transport-https -y
```

Example **Debian 11 (Bullseye)** source list:

```plaintext
deb cdrom:[Debian GNU/Linux 11.6.0 _Bullseye_ Official amd64 NETINST] bullseye main
deb http://deb.debian.org/debian/ bullseye main
deb-src http://deb.debian.org/debian/ bullseye-updates main
```

Example **Debian 12 (Bookworm)** source list:

```plaintext
deb https://deb.debian.org/debian/ bookworm main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware
```

---

### 11. Install Useful Utilities

```bash
apt install nano fonts-lato fonts-open-sans fonts-roboto fonts-mononoki fonts-indic zsh net-tools curl wget unzip -y
```

Reconnect if needed:

```bash
ssh root@192.168.1.21
```

---

### 12. Install Zsh and Plugins

Check current shell:

```bash
echo $SHELL
```

View available shells:

```bash
cat /etc/shells
```

Install Zsh and plugins:

```bash
apt install zsh zsh-autosuggestions zsh-syntax-highlighting -y
```

Check version and path:

```bash
zsh --version
which zsh
```

---

### 13. Set Zsh as Default Shell

Set for current user:

```bash
chsh -s $(which zsh)
```

Verify:

```bash
grep zsh /etc/passwd
```

---

### 14. Configure Zsh

Edit Zsh config:

```bash
nano /root/.zshrc
source ~/.zshrc
```

For another user:

```bash
su - nikhil
chsh -s $(which zsh)
nano /home/nikhil/.zshrc
```

---

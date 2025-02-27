

### 🔥 **Basic Firewalld Control Commands**  

| Command | Description |
|---------|------------|
| `systemctl start firewalld` | Start Firewalld |
| `systemctl stop firewalld` | Stop Firewalld |
| `systemctl restart firewalld` | Restart Firewalld |
| `systemctl enable firewalld` | Enable Firewalld at system startup |
| `systemctl disable firewalld` | Disable Firewalld at system startup |
| `systemctl status firewalld` | Check Firewalld status |
| `firewall-cmd --state` | Check if Firewalld is running |

---

### 🔥 **Zone Management Commands**  

| Command | Description |
|---------|------------|
| `firewall-cmd --get-zones` | List available zones |
| `firewall-cmd --get-default-zone` | Get the default zone |
| `firewall-cmd --set-default-zone=public` | Change the default zone |

---

### 🔥 **Adding and Removing Rules**  

| Command | Description |
|---------|------------|
| `firewall-cmd --list-all` | View all settings for the active zone |
| `firewall-cmd --zone=public --add-port=80/tcp --permanent` | Open HTTP port 80 permanently |
| `firewall-cmd --zone=public --remove-port=80/tcp --permanent` | Close HTTP port 80 permanently |
| `firewall-cmd --zone=public --add-service=http --permanent` | Allow HTTP service |
| `firewall-cmd --zone=public --remove-service=http --permanent` | Remove HTTP service |
| `firewall-cmd --reload` | Reload firewall rules |

---

### 🔥 **Interface and Service Management**  

| Command | Description |
|---------|------------|
| `firewall-cmd --get-active-zones` | Show active zones |
| `firewall-cmd --zone=public --add-interface=eth0` | Add `eth0` to a zone |
| `firewall-cmd --get-services` | List available services |
| `firewall-cmd --zone=public --add-service=ssh --permanent` | Allow SSH service |
| `firewall-cmd --zone=public --remove-service=ssh --permanent` | Remove SSH service |

---

### 🔥 **Masquerading and Port Forwarding**  

| Command | Description |
|---------|------------|
| `firewall-cmd --zone=public --add-masquerade --permanent` | Enable masquerading (for NAT) |
| `firewall-cmd --zone=public --remove-masquerade --permanent` | Disable masquerading |
| `firewall-cmd --zone=public --add-forward-port=port=8080:proto=tcp:toport=80` | Set up port forwarding |

---

### 🔥 **Advanced Rich Rules**  

| Command | Description |
|---------|------------|
| `firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.100" reject' --permanent` | Block IP `192.168.1.100` |
| `firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.100" accept' --permanent` | Allow IP `192.168.1.100` |

---

### 🔥 **Reset and Reload Firewalld**  

| Command | Description |
|---------|------------|
| `firewall-cmd --complete-reload` | Completely reload Firewalld (removes temporary rules) |
| `firewall-cmd --permanent --remove-service=http` | Remove HTTP service permanently |
| `firewall-cmd --runtime-to-permanent` | Save runtime changes permanently |

---

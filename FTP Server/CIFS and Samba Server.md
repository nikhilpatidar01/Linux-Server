
# **CIFS and Samba Server/Client Setup Guide**

---

## **Introduction to CIFS and Samba**

- **CIFS (Common Internet File System):** A protocol for file sharing and authentication over networks.
- **Samba Server:** Enables communication and file sharing between Windows and Linux/Unix systems.
- **Authentication:** Unlike FTP and HTTPD, which use an `htpasswd` file, Samba uses its own authentication mechanism.

---

## **Samba Client Setup (CentOS as Client, Windows as Server)**

### **Windows Setup**
1. **Create and share a folder** (e.g., `data/`).
2. **Check the Windows machine's IP address:**
   ```bash
   ipconfig
   ```
   - Displays network configuration, including the IP address.

---

### **CentOS Setup**

#### **Install Samba Client**

1. **Check if Samba is installed:**
   ```bash
   rpm -qa | grep samba
   ```
   - Lists all installed Samba-related packages.

2. **Install Samba Client:**
   ```bash
   yum install samba-client -y
   ```
   - Installs the Samba client package.

#### **Verify Installation**

1. **Check package details:**
   ```bash
   rpm -qi samba
   ```

2. **Check configuration files:**
   ```bash
   rpm -qc samba
   ```

3. **List installed files:**
   ```bash
   rpm -ql samba
   ```

4. **View documentation:**
   ```bash
   rpm -qd samba
   ```

#### **Available Samba Tools**
- `smbget`: Command-line tool for downloading files from SMB shares.  
- `smbtar`: Creates backups of SMB shares.  
- `smbtree`: Lists all available SMB shares on the network.

---

### **Connecting to Windows Share**

#### **Check Network Connectivity**
```bash
ping 192.168.1.55
```
- Verifies that the Windows server is reachable.

#### **List Shared Folders on Windows**
```bash
smbclient -L 192.168.1.55
smbclient -L //192.168.1.55
smbclient -L 192.168.1.55 -U <username>
```
- Displays available shared folders.

#### **Access the Shared Folder**
```bash
smbclient //192.168.1.55/data -U <username>
```
- Connects to the shared folder `data`.

#### **Download Files Using smbget**
```bash
smbget -r smb://192.168.1.55/data/index.html
```
- Downloads `index.html` recursively.

#### **Backup Using smbtar**
```bash
smbtar -s 192.168.1.55 -x data -u <username> -p <password> -t backup.tar
```
- Creates a backup of the `data` folder.

##### **Extract Backup**
```bash
tar -xvf backup.tar
```
- Extracts files from `backup.tar`.

---

## **Mount Windows Share Using CIFS**
```bash
mount -t cifs //192.168.1.55/data /mnt -o username=<user>,password=<password>
```
- Mounts the Windows share to `/mnt`.

---

## **Samba Server Setup (CentOS as Server, Windows as Client)**

### **Install Samba Server**
```bash
yum install samba -y
```
- Installs the Samba server package.

### **Start and Enable Samba Service**
```bash
systemctl start smb
systemctl enable smb
systemctl status smb
```
- Starts, enables, and checks the status of the Samba service.

### **Check Running Ports**
```bash
netstat -nltup | grep smb
```
- Verifies that Samba is running on ports 139 and 445.

### **Allow Samba Ports in Firewall**
```bash
firewall-cmd --permanent --add-service=samba
firewall-cmd --reload
```
- Opens firewall rules for Samba.

---

## **Accessing Samba Share from Windows**
1. Press `Win + R`.
2. Enter:
   ```
   \\192.168.1.31
   ```
3. Enter Samba username and password when prompted.

---

## **Creating Samba Users**
```bash
smbpasswd -a s1
smbpasswd -a u1
smbpasswd -a u2
```
- Adds new Samba users.

#### **List Samba Users**
```bash
pdbedit -L
```
- Displays all Samba users.

---

## **Setting Up a Common Shared Directory**

#### **Create a Common Directory**
```bash
mkdir /opt/data
chmod 777 /opt/data
```
- Creates a shared directory with full access permissions.

#### **Edit Samba Configuration**
```bash
nano /etc/samba/smb.conf
```
- Add the following to the end of the file:
  ```
  [common]
  path = /opt/data
  browsable = yes
  writable = yes
  valid users = armour, infosec
  ```

#### **Restart Samba**
```bash
systemctl restart smb
```
- Applies configuration changes.

---

## **Setting Up an Anonymous Public Share**

#### **Create a Public Directory**
```bash
mkdir /public
chmod -R 777 /public
chown -R nobody:nobody /public
```
- Creates a publicly accessible folder.

#### **Edit Samba Configuration**
```bash
nano /etc/samba/smb.conf
```
- Add the following:
  ```
  [public]
  path = /public
  browsable = yes
  writable = yes
  guest ok = yes
  ```

#### **Restart Samba**
```bash
systemctl restart smb
```

### **Access from Linux**
```bash
smbclient //192.168.1.31/public -N
```
- Connects without authentication.

### **Access from Windows**
```
\\192.168.1.31\public
```

---

## **Conclusion**
- **Samba enables cross-platform file sharing between Windows and Linux.**  
- **User authentication is managed with `smbpasswd` and `smb.conf`.**  
- **You can configure both authenticated and anonymous shares depending on your use case.**

---

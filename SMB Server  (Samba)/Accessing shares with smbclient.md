
# SMB Client
SMB client tools are utilities used to access and interact with SMB/CIFS file shares from a client machine, typically Linux or Unix systems. These tools allow you to connect to, browse, and transfer files to and from SMB servers (like Windows or Samba servers).
---


##  1. Package Management.
### Install Samba Client
```bash
yum install samba-client
```

### Verify Installation
 - Verification Installation
```bash
rpm -qa | grep samba
```
```bash
rpm -qa | grep samba-client
```
- Package information
```bash
rpm -qi grep samba-client
```
 - Docs Provided by the package
```bash
rpm -qd grep samba-client
```
- List all installed files
```bash
rpm -ql grep samba-client
```

## Browsing Shares with smbtree
### Discover Network Shares
```bash
smbtree
```
- Show only shares
```bash
smbtree -b 
```

- Authenticate as user 'win7'
```bash
smbtree -U win7
```
- Show a specific workgroup
```bash
smbtree -D workgroup
```
- List all available workgroups
```bash
smbtree -D workgroups
```


## Accessing shares with smbclient
smbclient works like an FTP client to access remote shares.
### Explore Available Shares

```bash
smbclient
```
- Help Show
```bash
- smbclient --help
```
- List Shares Anonymously
```bash
smbclient -L 192.168.112.145
```
- UNC Formate
 ```bash
smbclient -L //192.168.112.50
```
- With Username
 ```bash
smbclient -L //192.168.112.50 -U win7
```
- Guest Access (no Password)
 ```bash
smbclient -L //192.168.112.50 -U "" -N
```

---

## Connect to a Share
```bash
smbclient //192.168.112.145/data 
smbclient //192.168.112.145/data -U username
```
- Once Connected.

```bash
get <filename>
mget <filename1> <filename2> <filename3>

put <filename>
mput <filename1> <filename2> <filename3>
```

---

## 📂 3. Connect to a Remote Share (Interactive Shell)

```bash
smbclient //server-ip/Sharename -U username
```

Example:
```bash
smbclient //192.168.1.100/SharedFiles -U user1
```

Inside the `smb:` prompt, you can use:
- `ls` — list files
- `cd` — change directory
- `get filename` — download file
- `put filename` — upload file
- `exit` — quit

---

## 🔁 4. Mount SMB Share to Local Directory

Create a mount point:
```bash
sudo mkdir -p /mnt/smb_share
```

### Mount with Credentials:
```bash
sudo mount -t cifs //server-ip/Sharename /mnt/smb_share -o username=your_user,password=your_pass,iocharset=utf8
```

### Example:
```bash
sudo mount -t cifs //192.168.1.100/SharedFiles /mnt/smb_share -o username=user1,password=yourpass
```

> ✅ Use `vers=3.0`, `vers=2.1`, etc. if needed:  
```bash
-o username=user1,password=yourpass,vers=3.0
```

---

## 🔐 5. Store Credentials Securely

Create a file:
```bash
sudo nano /etc/samba/credentials
```

Add:
```
username=your_user
password=your_pass
```

Set permissions:
```bash
sudo chmod 600 /etc/samba/credentials
```

Mount using:
```bash
sudo mount -t cifs //192.168.1.100/SharedFiles /mnt/smb_share -o credentials=/etc/samba/credentials,iocharset=utf8
```

---

## 📌 6. Persistent Mount via `/etc/fstab`

Add to `/etc/fstab`:
```fstab
//192.168.1.100/SharedFiles /mnt/smb_share cifs credentials=/etc/samba/credentials,iocharset=utf8,uid=1000,gid=1000 0 0
```

Reload mounts:
```bash
sudo mount -a
```

---

## 🧪 7. Troubleshooting

- View mount errors:
  ```bash
  dmesg | grep CIFS
  journalctl -xe
  ```

- Check required kernel support:
  ```bash
  modinfo cifs
  ```

---

## 📦 Common Tools Summary

| Tool          | Purpose                              |
|---------------|--------------------------------------|
| `smbclient`   | CLI tool to connect to SMB shares    |
| `cifs-utils`  | Provides CIFS mount support          |
| `nmblookup`   | Find NetBIOS names on the network    |
| `smbtree`     | Graphically browse SMB network shares|

---


# 📁 SMB Client

This document provides commands and usage examples for SMB/CIFS client utilities on Linux (especially RHEL-based distros). It covers package management, share discovery, file transfer, and mounting via CIFS using tools like `smbclient`, `smbtree`, `smbget`, and `smbtar`.

---

## 📦 Package Management

### 🔧 Install Samba Client
```bash
yum install samba-client
```

### ✅ Verify Installation

- Check installed packages:
```bash
rpm -qa | grep samba
```
```bash
rpm -qa | grep samba-client
```

- Get package information:
```bash
rpm -qi samba-client
```

- View documentation provided by the package:
```bash
rpm -qd samba-client
```

- List all installed files from the package:
```bash
rpm -ql samba-client
```

---

## 🌐 Browsing Shares with `smbtree`

### 🔍 Discover Network Shares
```bash
smbtree
```

- Show only shares:
```bash
smbtree -b 
```

- Authenticate as user 'win7':
```bash
smbtree -U win7
```

- Show a specific workgroup:
```bash
smbtree -D workgroup
```

- List all available workgroups:
```bash
smbtree -D workgroups
```

---

## 📂 Accessing Shares with `smbclient`

`smbclient` works like an FTP client to access remote SMB shares.

### 🔎 Explore Available Shares
```bash
smbclient
```

- Show help:
```bash
smbclient --help
```

- List shares anonymously:
```bash
smbclient -L 192.168.112.145
```

- Use UNC format:
```bash
smbclient -L //192.168.112.50
```

- Connect with username:
```bash
smbclient -L //192.168.112.50 -U win7
```

- Guest access (no password):
```bash
smbclient -L //192.168.112.50 -U "" -N
```

---

## 🔗 Connect to a Share

```bash
smbclient //192.168.112.145/data
smbclient //192.168.112.145/data -U username
```

### 📥 Once Connected

- Download files:
```bash
get <filename>
mget <filename1> <filename2> <filename3>
```

- Upload files:
```bash
put <filename>
mput <filename1> <filename2> <filename3>
```

---

## 📥 Download with `smbget`

`smbget` allows file downloads from SMB shares via command line, similar to `wget`.

- Example:
```bash 
smbget -U Nikhil smb://192.168.112.145/data/0S2/VBoxReplaceDLL.exe
```

---

## 🗃️ Backup with `smbtar`

`smbtar` backs up or restores entire SMB shares using the TAR format.

- Show help:
```bash
smbtar --help
```

- Backup example:
```bash
smbtar -s 192.168.112.145 -x data -U Nikhil -p 123 -t data.tar -v
```

- Option meanings:
  - `-s`: Server address  
  - `-x`: Share name  
  - `-U / -p`: Username and password  
  - `-t`: Output TAR archive  
  - `-v`: Verbose output

---

## 📌 Mounting Shares with `mount.cifs`

Linux systems can mount SMB shares into the filesystem using the CIFS kernel module.

- Mount share to `/mnt`:
```bash
mount -t cifs -o username=win10,password=123 //192.168.112.150/data /mnt
```

- Mount another share to a custom directory:
```bash
mount -t cifs -o username=Nikhil,password=123 //192.168.112.150/data /mnt/d1
```

- Unmount:
```bash
umount /mnt
```

---

## 📝 Summary & Notes

### ✅ Summary:
- **smbclient**: Interacts with SMB shares like an FTP client.
- **smbtree**: Discovers and lists available SMB shares.
- **smbget**: Downloads files from SMB shares like wget.
- **smbtar**: Backs up entire SMB shares in tar format.
- **mount.cifs**: Mounts SMB shares to the Linux filesystem.

### 📌 Notes:
- Always ensure the **`cifs-utils`** package is installed for mounting.
- Use `-U` to specify a username when required.
- Use secure methods (e.g., `credentials` file) to avoid exposing passwords in commands.
- CIFS version may be needed in some cases: `vers=1.0`, `vers=2.1`, etc.

---


# SMB Client
This Document provides commands and usage exapmles for SMB/CIFS client utilities on Linux (especially RHEL-based distros). it covers package management, share discovery, file transfe and mounting via CIFS using tools like 'smbclient', 'smbtree', 'smbget' and 'smbtar'.
---


## Package Management.
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
### Once Connected.
 - Downlaod Files
```bash
get <filename>
mget <filename1> <filename2> <filename3>
```
 - Insert Files
```bash
put <filename>
mput <filename1> <filename2> <filename3>
```

---
## Download with smbget
- smbget allow file downloads from SMB shares via command line, similar to wget.
```bash 
smbget -U Nikhil smb://192.168.112.145/data/0S2/VBoxReplaceDLL.exe
```
Download a file using SMB credential and a full path.
---

## Backup with smbtar
 smbtar backs up or restores entire SMB shares using the tar forate.
- smbtar help
```bash
smbtar --help
```
- Displays usage for smbtar.
```bash
smbtar -s 192.168.112.145 -x data -U Nikhil -p 123 -t data.tar -v
```
- -s : serveraddress
- -x : Share Name
- -U / -p : Username and Password 
- -t : Output tar archive
- -v : Verbose output
  
---

## Mounting shares with mount.cifs
- Linux system can mount SMB shares into the filesystem using the cifs kernel module. 
```bash
mount -t cifs -o username=win10,password=123 //192.168.112.150/data /mnt
```
- Mounts a share to /mnt using specified credentials.
```bash
mount -t cifs -o username=Nikhil,password=123 //192.168.112.150/data /mnt/d1
```

- Mounts another share to a custom direcoty.
```bash
umount /mnt
```


---

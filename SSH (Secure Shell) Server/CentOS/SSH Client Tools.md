
## 🔐SSH Client Tools💻

1. **SSH**: Secure remote access to servers. 🔒  
   - Example: `ssh user@hostname`

2. **SCP**: Secure file transfer between local and remote systems. 📁  
   - Example: `scp file.txt user@hostname:/remote/directory`

3. **SFTP**: Interactive file transfer over SSH. 🔄  
   - Example: `sftp user@hostname`

4. **RSYNC**: Sync files and directories securely between systems. 🔄🔐  
   - Example: `rsync -avz /local/dir user@hostname:/remote/dir`

5. **SSH Keys**: Manage public/private key pairs for passwordless login. 🗝️  
   - Example: `ssh -i ~/.ssh/id_rsa user@hostname`

---

## 1. Basic SSH Usage 🔹

### 🔸 To connect to a remote system using the default username and port:  
```bash
ssh 192.168.1.43
```

---

### 🔸 To connect as a specific user on a remote system:  
```bash
ssh -t root@192.168.1.31
```
```bash
ssh root@192.168.1.34
```

---

### 🔸 To specify a custom port for SSH (if the default port 22 has been changed):  
```bash
ssh -l root -p 2222 192.168.1.34
```
```bash
ssh -p 2200 user@192.168.1.50
```

---

### 🔸 To authenticate using a private key file (for secure login instead of a password):  
```bash
ssh -i id_rsa_centos root@192.168.1.34
```
```bash
ssh -i ~/.ssh/id_ecdsa root@192.168.1.34
```

---

### 🔸 To enable verbose/debug mode for troubleshooting SSH connection:  
```bash
ssh -v root@192.168.1.34
```
```bash
ssh -vvv -i ~/.ssh/id_rsa root@192.168.1.34
```

---

### 🔸 To execute a single command on the remote server without logging in:  
```bash
ssh root@192.168.1.34 -p 22 id
```

---

### 🔸 To check the uptime (system running time) of a remote system:  
```bash
ssh root@192.168.1.34 'uptime'
```

---

### 🔸 To read the hostname file on a remote system with an identity file and port:  
```bash
ssh root@192.168.1.34 -p 22 -i ~/.ssh/id_rsa 'cat /etc/hostname'
```

---

### 🔸 To check the disk space usage on a remote machine:  
```bash
ssh -i ~/.ssh/id_rsa root@192.168.1.34 "df -h"
```

---

### 🔸 To locally tunnel a remote server's web port from your local system:  
```bash
ssh -L 8080:localhost:80 root@192.168.1.34
```

---

### 🔸 To connect to an internal machine through a gateway (jump host):  
```bash
ssh -J root@gateway root@192.168.1.34
```

---

## 2. Secure Copy (SCP)🔹

---

### 🔸 To send a single file to a specific directory on the remote server:  
```bash
scp file.txt root@192.168.1.34:/root/
```
```bash
scp ftp-server.py root@192.168.1.31:/tmp/
```

---

### 🔸 To transfer multiple files at once to the remote system:  
```bash
scp file1.txt file2.txt root@192.168.1.34:/home/root/
```
```bash
scp image.png notes.md root@192.168.1.100:/var/www/html/
```

---

### 🔸 To recursively copy an entire directory and its contents from the local system to the remote system:  
```bash
scp -r /local/dir root@192.168.1.34:/remote/dir
```
```bash
scp -r ~/Downloads root@192.168.1.34:/tmp/
```

---

### 🔸 To copy a file or folder from the remote server to the local machine:  
```bash
scp root@192.168.1.34:/root/file.txt ./local/dir/
```
```bash
scp -r root@192.168.1.34:/var/www/html/ /tmp/
```

---

### 🔸 To set a bandwidth limit during transfer (useful for slow networks):  
```bash
scp -l 500 file.zip root@192.168.1.20:/tmp/
```

---

## 3. Rsync Commands 🔹

---

### 🔸 To install the Rsync tool on CentOS or RHEL system:  
```bash
yum install rsync
```

---

### 🔸 To sync a directory from a remote server to the local machine (for backup purposes):  
```bash
rsync -av root@192.168.1.34:/var/log /tmp/
```
```bash
rsync -az root@192.168.1.34:/var/log /tmp/
```

---

### 🔸 To upload files or a directory from the local system to the remote server:  
```bash
rsync -av /tmp/log root@192.168.1.34:/var/
```
```bash
rsync -az /tmp/log root@192.168.1.34:/var/
```

---

### 🔸 To securely perform an rsync transfer using a private key file:  
```bash
rsync -av -e 'ssh -i ~/.ssh/id_rsa' root@192.168.1.34:/var/log/ /tmp/log
```

---

### 🔸 To run an rsync command on a non-default port:  
```bash
rsync -av -e 'ssh -p 2200' /backup root@192.168.1.100:/mnt/backup
```

---

### 🔸 To perform a dry-run with rsync (preview the output without actual transfer):  
```bash
rsync -av --dry-run ~/Documents root@192.168.1.34:/var/log/tmp/
```

---

### 🔸 To mirror a remote directory exactly as the local directory (extra files will be deleted):  
```bash
rsync -av --delete ~/projects root@192.168.1.34:/var/log/
```

---

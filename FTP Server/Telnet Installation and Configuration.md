
# **Telnet Installation and Configuration Guide**

---

## **Introduction**

**Telnet** is a network protocol that provides a command-line interface for communicating with a remote device over a TCP/IP network. While it's mostly replaced by SSH due to its lack of encryption, Telnet is still useful for:

- Debugging and testing network services.
- Accessing legacy systems.
- Lightweight remote control where security is not a concern.

---

## **What is a Telnet Server?**

A **Telnet server** listens for incoming Telnet connections—typically on **TCP port 23**—and provides users with shell access to execute commands on the server.

---

## **How Does Telnet Work?**

1. The client initiates a connection to the Telnet server over TCP.
2. The server responds and prompts for login credentials.
3. After authentication, the client can execute commands remotely.
4. The session remains active until the user exits or disconnects.

---

## **Key Features of Telnet**

- ✅ **Remote Access** – Manage systems remotely via CLI.  
- ✅ **Lightweight** – Minimal system resource usage.  
- ❌ **No Encryption** – Transmits data, including passwords, in plain text.  
- 🔧 **Command Execution** – Run shell commands on remote machines.  
- 🧓 **Legacy Support** – Useful for systems where SSH isn't supported.

---

## **1. Install Telnet**

Check for existing Telnet packages and install if missing:

```bash
rpm -qa | grep telnet
yum install telnet* -y
rpm -qa | grep telnet
```

---

## **2. Verify Telnet Installation**

Check package info, configuration files, documentation, and installed files:

```bash
rpm -qi telnet-server
rpm -ql telnet-server
rpm -qd telnet-server
rpm -qc telnet-server
```

---

## **3. Configure Telnet Socket**

Create or edit the Telnet socket file:

```bash
vim /etc/systemd/system/telnet.socket
```

**Contents:**
```ini
[Unit]
Description=Telnet Server Activation Socket

[Socket]
ListenStream=23
Accept=yes

[Install]
WantedBy=sockets.target
```

---

## **4. Configure Telnet Service**

Create or edit the Telnet service template:

```bash
vim /etc/systemd/system/telnet@.service
```

**Contents:**
```ini
[Unit]
Description=Telnet Server Service

[Service]
ExecStart=-/usr/sbin/in.telnetd
StandardInput=socket
```

---

## **5. Verify Telnet Port Status**

Ensure that **port 23** is listening:

```bash
netstat -nltup | grep 23
```

---

## **6. Configure Firewall (iptables)**

Edit the iptables configuration file:

```bash
vim /etc/sysconfig/iptables
```

Add the following rule to allow incoming Telnet connections:

```bash
-A INPUT -p tcp -m tcp --dport 23 -j ACCEPT
```

---

## **7. Restart Services**

Reload systemd and apply changes:

```bash
systemctl daemon-reexec
systemctl restart telnet.socket
systemctl restart iptables.service
```

---

Here's an improved and **detailed version** of **Point 8: Using Telnet**, with proper structure, explanations, examples, and usage tips:

---

## **8. How to Use Telnet After Configuration**

### **1. Open a Terminal on the Client Machine**

You can use any terminal (Linux, macOS, or Windows Command Prompt if Telnet is enabled).

---

### **2. Connect to the Telnet Server**

Use the following command:

```bash
telnet <server-ip>
```

🔹 Replace `<server-ip>` with the actual IP address of the Telnet server.

**Example:**
```bash
telnet 192.168.1.100
```

> If the connection is successful, you'll see something like:
```
Trying 192.168.1.100...
Connected to 192.168.1.100.
Escape character is '^]'.
```

---

### **3. Login to the Remote System**

After connecting, you'll see a login prompt:

```
login:
```

- Enter the **valid username** on the Telnet server.
- Then enter the **password** when prompted.

> ⚠️ Note: Telnet transmits credentials in **plain text**. It is not secure and should only be used in trusted or isolated environments.

---

### **4. Run Remote Commands**

Once logged in, you’ll be placed in a shell (typically Bash):

```bash
$ whoami
$ hostname
$ uptime
$ ls -l /home
```

You can perform almost any operation as if you were sitting at that machine.

---

### **5. Verify Remote Access and Permissions**

Try reading or modifying files (if permitted):

```bash
cat /etc/os-release
cd /var/log
less messages
```

If you run into permission issues, make sure the account you're using has appropriate privileges.

---

### **6. Transfer Files (Limited Capability)**

Telnet by itself does **not support file transfers**, but you can copy/paste text or use `echo` and redirection for very small text files:

```bash
echo "Hello from remote client" > hello.txt
```

For actual file transfer, use FTP, SCP, or TFTP instead.

---

### **7. Disconnect from the Telnet Session**

To safely log out from the Telnet session:

```bash
exit
```

Or simply press:

```
Ctrl + D
```

This will close the remote session and return you to your local terminal.

---

### **8. Troubleshooting Connection Issues**

- **Connection Refused:** The server might not be listening on port 23. Use:
  ```bash
  netstat -tulpn | grep 23
  ```

- **Firewall Block:** Ensure port 23 is open in the server's firewall:
  ```bash
  iptables -L -n | grep 23
  ```

- **SELinux Restrictions:** If SELinux is enforcing, you may need to allow Telnet explicitly.

- **Service Not Running:** Restart Telnet service:
  ```bash
  systemctl restart telnet.socket
  ```

---

## ✅ Summary Practices for Using Telnet

| Action | Command | Notes |
|--------|---------|-------|
| Connect | `telnet <server-ip>` | Replace with actual IP |
| Authenticate | `login:` + `password:` | Uses system accounts |
| Run commands | `ls`, `cat`, etc. | Just like a local terminal |
| Exit | `exit` or `Ctrl+D` | Ends the session |
| Check service | `netstat -nltup | grep 23` | Ensure Telnet is active |

---


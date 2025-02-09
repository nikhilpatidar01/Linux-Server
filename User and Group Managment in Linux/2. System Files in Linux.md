# 🔐 **Linux User & Group Configuration Files Explained** 🐧  

Linux manages **users** and **groups** using several important configuration files. These files store essential information like **user accounts, passwords, groups, and security settings**. Understanding these files is **crucial** for system administration and security.  

This guide covers:  
1️⃣ **`/etc/passwd`** - Stores user account information.  
2️⃣ **`/etc/shadow`** - Stores encrypted passwords & security policies.  
3️⃣ **`/etc/group`** - Stores group information.  
4️⃣ **`/etc/gshadow`** - Stores secure group details.  

---

## 📜 **1. `/etc/passwd` – User Account Information**  
📌 **Purpose:** This file contains **basic details of all user accounts** in the system, including **username, UID, GID, home directory, and default shell**.  

📌 **Access:** **Public** (All users can read it).  

### 📌 **File Format:**
Each line represents a **user account** with the following format:  
```bash
username:x:UID:GID:user_info:home_directory:shell
```

### 📄 **Example Entry:**
```bash
john:x:1001:1001:John Doe:/home/john:/bin/bash
```
🔍 **Breakdown:**  
- **`john`** → Username  
- **`x`** → Placeholder for password (stored in `/etc/shadow`)  
- **`1001`** → User ID (UID)  
- **`1001`** → Group ID (GID)  
- **`John Doe`** → User description  
- **`/home/john`** → Home directory  
- **`/bin/bash`** → Default shell  

### 🛠️ **Commands to View & Modify:**
- **View all users:**  
  ```bash
  cat /etc/passwd
  ```
- **Add a new user:**  
  ```bash
  sudo useradd -m -s /bin/bash john
  ```
- **Modify user details:**  
  ```bash
  sudo usermod -c "New Info" john
  ```

---

## 🔒 **2. `/etc/shadow` – Secure Password Storage**  
📌 **Purpose:** Stores **encrypted passwords** and **password aging policies** for users.  

📌 **Access:** **Restricted** (Only root can read it).  

### 📌 **File Format:**
Each line represents a **user password entry** with the following format:  
```bash
username:encrypted_password:last_changed:min_age:max_age:warn:inactive:expire:reserved
```

### 📄 **Example Entry:**
```bash
john:$6$somehash:17542:0:99999:7::::
```
🔍 **Breakdown:**  
- **`john`** → Username  
- **`$6$somehash`** → Encrypted password  
- **`17542`** → Days since the last password change  
- **`0`** → Minimum days before changing password  
- **`99999`** → Maximum password validity days  
- **`7`** → Warning days before password expires  

### 🛠️ **Commands to Manage Passwords:**
- **View shadow file (Root Only):**  
  ```bash
  sudo cat /etc/shadow
  ```
- **Change a user’s password:**  
  ```bash
  sudo passwd john
  ```
- **Force a password reset after 30 days:**  
  ```bash
  sudo chage -M 30 john
  ```

---

## 👥 **3. `/etc/group` – Group Information**  
📌 **Purpose:** Stores **group names, GIDs, and group members**.  

📌 **Access:** **Public** (All users can read it).  

### 📌 **File Format:**
Each line represents a **group entry** in the following format:  
```bash
groupname:x:GID:member1,member2,member3
```

### 📄 **Example Entry:**
```bash
developers:x:1002:alice,bob,john
```
🔍 **Breakdown:**  
- **`developers`** → Group name  
- **`x`** → Placeholder (group password is in `/etc/gshadow`)  
- **`1002`** → Group ID (GID)  
- **`alice, bob, john`** → Members of the group  

### 🛠️ **Commands to Manage Groups:**
- **View all groups:**  
  ```bash
  cat /etc/group
  ```
- **Create a new group:**  
  ```bash
  sudo groupadd developers
  ```
- **Add a user to a group:**  
  ```bash
  sudo usermod -aG developers john
  ```

---

## 🔐 **4. `/etc/gshadow` – Secure Group Management**  
📌 **Purpose:** Stores **group passwords and administrator settings**.  

📌 **Access:** **Restricted** (Only root can read it).  

### 📌 **File Format:**
Each line represents a **secure group entry** in this format:  
```bash
groupname:encrypted_password:group_admins:group_members
```

### 📄 **Example Entry:**
```bash
developers:$6$somehash:alice,bob:alice,bob,john
```
🔍 **Breakdown:**  
- **`developers`** → Group name  
- **`$6$somehash`** → Encrypted password  
- **`alice,bob`** → Group administrators  
- **`alice,bob,john`** → Group members  

### 🛠️ **Commands to Modify Groups Securely:**
- **View gshadow file (Root Only):**  
  ```bash
  sudo cat /etc/gshadow
  ```
- **Set a group password:**  
  ```bash
  sudo gpasswd developers
  ```
- **Assign a group administrator:**  
  ```bash
  sudo gpasswd -A alice developers
  ```

---

## 🔍 **5. Summary Table**
| 📂 **File** | 📌 **Purpose** | 🔒 **Access** |
|------------|--------------|--------------|
| **`/etc/passwd`** | Stores user account information | Public |
| **`/etc/shadow`** | Stores encrypted passwords | Root Only |
| **`/etc/group`** | Stores group details | Public |
| **`/etc/gshadow`** | Stores secure group settings | Root Only |

---

## 🎯 **6. Key Takeaways**
✔ **Linux stores user & group details in `/etc/passwd` and `/etc/group`** 📁  
✔ **Passwords are securely encrypted in `/etc/shadow` and `/etc/gshadow`** 🔒  
✔ **Root user has exclusive access to `/etc/shadow` and `/etc/gshadow`** 🛑  
✔ **Users & groups can be managed using commands like `useradd`, `passwd`, and `groupadd`** ⚙️  

---

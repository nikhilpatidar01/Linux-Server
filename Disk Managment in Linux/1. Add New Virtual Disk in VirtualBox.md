# 📌 **How to Add a New Virtual Disk in VirtualBox (VM Box) – Step-by-Step Guide**  

Adding a new virtual disk to a **VirtualBox Virtual Machine (VM)** allows you to expand storage without reinstalling the OS.  

✅ **Works for:** Windows, Linux, macOS Virtual Machines  
✅ **Covers:** Adding a disk, formatting, and mounting  

---

# 🖥 **1. Add a New Virtual Disk to the VM in VirtualBox**  

### 🔹 **Step 1: Open VirtualBox and Select Your VM**
1️⃣ **Open VirtualBox**  
2️⃣ **Select the Virtual Machine** (e.g., Ubuntu, CentOS, Windows)  
3️⃣ Click on **Settings (⚙️ icon)**  

---

### 🔹 **Step 2: Add a New Virtual Disk**
1️⃣ Go to **Storage → Controller: SATA**  
2️⃣ Click **Add Hard Disk (📂 icon with a + sign)**  
3️⃣ Select **Create a New Disk**  
4️⃣ Choose **VDI (VirtualBox Disk Image)** → Click **Next**  
5️⃣ Choose **Dynamically Allocated** → Click **Next**  
6️⃣ Set the disk size (e.g., **20GB**) → Click **Create**  

📌 **Result:** A new virtual disk (e.g., `vdb`) is added to the VM.  

---

### 🔹 **Step 3: Boot the VM**
1️⃣ **Start the VM**  
2️⃣ Open a **terminal (Linux) or Disk Management (Windows)**  
3️⃣ Run the following command to check the new disk:  

```bash
lsblk
```
📌 **Output Example:**
```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0   50G  0 disk /
vdb      8:16   0   20G  0 disk 
```
✅ **New disk (`vdb`) is detected.**  

---

# 📌 **2. Partition, Format, and Mount the Disk in Linux**  

### 🔹 **Step 1: Create a Partition (`fdisk`)**  
```bash
sudo fdisk /dev/vdb
```
📌 **Inside `fdisk`:**  
```bash
n   # Create new partition
p   # Primary partition
1   # Partition number 1
ENTER  # Default first sector
ENTER  # Default last sector (uses full disk)
w   # Write changes & exit
```

---

### 🔹 **Step 2: Format the Partition (`ext4`)**
```bash
sudo mkfs.ext4 /dev/vdb1
```
📌 **Output Example:**
```
Creating filesystem with 5242880 4k blocks and 1310720 inodes
```
✅ **Formats `/dev/vdb1` as `ext4`.**  

---

### 🔹 **Step 3: Create a Mount Point & Mount the Disk**
```bash
sudo mkdir /mnt/newdisk
sudo mount /dev/vdb1 /mnt/newdisk
```
✅ **Mounts the new disk at `/mnt/newdisk`.**  

---

### 🔹 **Step 4: Verify the Mounted Disk (`df -h`)**
```bash
df -h
```
📌 **Output Example:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/vdb1       20G  1.0G  19G   5% /mnt/newdisk
```
✅ **New disk is successfully mounted!** 🎉  

---

# 📌 **3. Auto-Mount the Disk at Boot (`/etc/fstab`)**  

### 🔹 **Step 1: Get the UUID of the New Disk**
```bash
sudo blkid
```
📌 **Example Output:**
```
/dev/vdb1: UUID="1234-abcd-5678-efgh" TYPE="ext4"
```

---

### 🔹 **Step 2: Edit `/etc/fstab` to Auto-Mount**
```bash
sudo nano /etc/fstab
```
🔹 **Add this line at the end:**
```
UUID=1234-abcd-5678-efgh  /mnt/newdisk  ext4  defaults  0  2
```
✅ **Ensures the disk mounts automatically on boot.**  

---

### 🔹 **Step 3: Apply Changes & Test**
```bash
sudo mount -a
```
✅ **Confirms `fstab` changes.**  

---

# 📌 **4. Verify Everything**  

### 🔹 **Check Disk Space**
```bash
lsblk
df -h
```
✅ **Ensures the disk is properly mounted.**  

### 🔹 **Reboot & Test**
```bash
sudo reboot
```
✅ **After reboot, check if `/mnt/newdisk` is still available.**  

---

# 🚀 **Summary Table**
| **Step** | **Command** | **Purpose** |
|---------|------------|------------|
| **1. Add Disk in VirtualBox** | VirtualBox → Storage → Add New Disk | Create virtual disk |
| **2. Detect New Disk** | `lsblk` | Check if new disk appears |
| **3. Partition Disk** | `fdisk /dev/vdb` | Create partition |
| **4. Format Disk** | `mkfs.ext4 /dev/vdb1` | Format as `ext4` |
| **5. Mount Disk** | `mount /dev/vdb1 /mnt/newdisk` | Temporary mount |
| **6. Auto-Mount on Boot** | Edit `/etc/fstab` | Permanent mount |
| **7. Verify & Reboot** | `df -h`, `lsblk`, `reboot` | Check & confirm |

---

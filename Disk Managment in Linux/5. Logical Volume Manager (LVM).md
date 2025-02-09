# **🔹 Logical Volume Manager (LVM) in Linux 🔹**  

LVM (**Logical Volume Manager**) allows **dynamic disk management**, making it easier to resize, extend, and manage storage efficiently in Linux. It is **better than traditional partitions** because you can increase, shrink, and move volumes without unmounting them.  

✅ **Works on:** RHEL, CentOS, Ubuntu, Debian, Fedora  
✅ **Features:** Resize partitions **without losing data**, **create snapshots**, add **multiple disks** into a single volume  

---

# **📌 Key Concepts of LVM**  

### **1️⃣ Physical Volume (PV)**  
A **physical volume (PV)** is a hard disk or partition used for LVM.  

### **2️⃣ Volume Group (VG)**  
A **volume group (VG)** is a collection of **one or more physical volumes** (PVs).  

### **3️⃣ Logical Volume (LV)**  
A **logical volume (LV)** is a partition inside a volume group, similar to a normal partition but flexible.  

📌 **Diagram: How LVM Works**  

```
+-------------------------------+
|  Physical Disk (sda, sdb)     |
+-------------------------------+
|  Physical Volumes (PVs)       | ← Create PVs
+-------------------------------+
|  Volume Group (VG)            | ← Create VG
+-------------------------------+
|  Logical Volumes (LV)         | ← Create LV
+-------------------------------+
|  Filesystem (ext4, xfs)       | ← Format and Mount
+-------------------------------+
```

---

# **🚀 Step-by-Step Guide to Setting Up LVM**  

## **🛠 Step 1: Identify Available Disks**  

First, check available **physical disks**.  

```bash
lsblk
```

📌 **Example Output:**  
```
NAME   MAJ:MIN  RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  100G  0 disk  
sdb      8:16   0   50G  0 disk  
sdc      8:32   0   50G  0 disk  
```

**🟢 Here, `sdb` and `sdc` are free disks we can use for LVM.**  

---

## **🛠 Step 2: Create Physical Volumes (PVs)**  

Convert **sdb** and **sdc** into LVM **physical volumes (PV)**.  

```bash
sudo pvcreate /dev/sdb /dev/sdc
```

📌 **Output:**  
```
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.
```

✅ **Now, `sdb` and `sdc` are ready for LVM!**  

---

## **🛠 Step 3: Create a Volume Group (VG)**  

Now, create a **volume group** named `my_vg` using **sdb** and **sdc**.  

```bash
sudo vgcreate my_vg /dev/sdb /dev/sdc
```

📌 **Output:**  
```
  Volume group "my_vg" successfully created
```

✅ **Now, both `sdb` and `sdc` are combined into a single volume group named `my_vg`.**  

---

## **🛠 Step 4: Verify Volume Group**  

To check volume groups:  

```bash
sudo vgdisplay
```

📌 **Example Output:**  
```
  VG Name               my_vg
  VG Size               99.99 GiB
  Free PE / Size        99.99 GiB
```

✅ **Volume group `my_vg` has `99.99 GiB` available for use.**  

---

## **🛠 Step 5: Create a Logical Volume (LV)**  

Now, create a **logical volume** (`my_lv`) of **20GB** inside `my_vg`.  

```bash
sudo lvcreate -L 20G -n my_lv my_vg
```

📌 **Output:**  
```
  Logical volume "my_lv" created.
```

✅ **A logical volume `my_lv` (20GB) is created inside `my_vg`.**  

---

## **🛠 Step 6: Format the Logical Volume**  

Before using the logical volume, format it with `ext4` or `xfs`.  

```bash
sudo mkfs.ext4 /dev/my_vg/my_lv
```

📌 **Output:**  
```
  Creating filesystem with 5242880 blocks and 1310720 inodes
```

✅ **`my_lv` is now formatted with ext4.**  

---

## **🛠 Step 7: Mount the Logical Volume**  

Create a directory and mount the volume.  

```bash
sudo mkdir /mnt/mydata
sudo mount /dev/my_vg/my_lv /mnt/mydata
```

✅ **The volume is now mounted at `/mnt/mydata`.**  

---

## **🛠 Step 8: Make the Mount Permanent**  

Edit `/etc/fstab` to **automount the LVM** after reboot.  

```bash
sudo nano /etc/fstab
```

🔹 **Add this line:**  
```plaintext
/dev/my_vg/my_lv  /mnt/mydata  ext4  defaults  0 0
```

✅ **Now, the logical volume mounts automatically on boot.**  

---

# **🔥 Advanced LVM Operations 🔥**  

## **📌 Extend a Logical Volume (Increase Size)**  

### **1️⃣ Check Free Space**  
```bash
sudo vgdisplay my_vg
```

📌 **Example Output:**  
```
  Free PE / Size       79.99 GiB
```
✅ **`my_vg` has `79.99 GiB` free.**

---

### **2️⃣ Extend the Logical Volume (`+10G`)**  

Increase `my_lv` by **10GB**:  

```bash
sudo lvextend -L +10G /dev/my_vg/my_lv
```

✅ **New size is `30GB`.**

---

### **3️⃣ Resize the Filesystem**  

```bash
sudo resize2fs /dev/my_vg/my_lv
```

📌 **Output:**  
```
Filesystem on /dev/my_vg/my_lv is now 30G
```

✅ **Now, `my_lv` is `30GB`.**

---

## **📌 Reduce the Size of a Logical Volume (Decrease Size)**  

⚠ **Be Careful!** Shrinking a volume **can cause data loss**.  

1️⃣ **First, unmount the volume:**  
```bash
sudo umount /mnt/mydata
```

2️⃣ **Resize the filesystem (reduce to 15GB):**  
```bash
sudo resize2fs /dev/my_vg/my_lv 15G
```

3️⃣ **Reduce the logical volume:**  
```bash
sudo lvreduce -L 15G /dev/my_vg/my_lv
```

4️⃣ **Remount the volume:**  
```bash
sudo mount /dev/my_vg/my_lv /mnt/mydata
```

✅ **Now, `my_lv` is `15GB`.**

---

## **📌 Take a Snapshot of a Logical Volume**  

1️⃣ **Create a snapshot (backup) of `my_lv`:**  

```bash
sudo lvcreate -L 5G -s -n my_lv_snapshot /dev/my_vg/my_lv
```

📌 **Now you have a snapshot of `my_lv` that can be restored later!**  

---

# **📊 Summary of LVM Commands**  

| Command | Description |
|---------|-------------|
| `lsblk` | Show all available disks |
| `pvcreate /dev/sdb` | Create a physical volume |
| `vgcreate my_vg /dev/sdb` | Create a volume group |
| `lvcreate -L 20G -n my_lv my_vg` | Create a logical volume |
| `mkfs.ext4 /dev/my_vg/my_lv` | Format a logical volume |
| `mount /dev/my_vg/my_lv /mnt/data` | Mount a logical volume |
| `lvextend -L +10G /dev/my_vg/my_lv` | Extend a logical volume |
| `resize2fs /dev/my_vg/my_lv` | Resize the filesystem |
| `lvremove /dev/my_vg/my_lv` | Delete a logical volume |

---

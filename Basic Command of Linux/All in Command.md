
# 🖥️ Linux Basic Commands Cheat Sheet

## 📛 Hostname & Current Directory

```bash
hostname          # Shows the system's hostname.
pwd               # Print current working directory.
```

## 📂 Listing Files

```bash
ls                # List directory contents.
ls -l             # Long listing format.
ls -lh            # Long listing with human-readable file sizes.
ls --help         # Show help for the 'ls' command.
info ls           # Show detailed documentation for 'ls'.
man ls            # Manual page for 'ls'.
```

## 📁 Navigating Directories

```bash
cd .              # Stay in the current directory.
cd ..             # Move up one level.
cd ../..          # Move up two levels.
```

## 🐍 Python Version

```bash
python --version  # Display installed Python version.
```

## 📄 Viewing and Creating Files

```bash
cat n.txt         # View contents of 'n.txt'.
cat > n.txt       # Create/overwrite 'n.txt' (Ctrl+D to save).
cat >> n.txt      # Append text to 'n.txt' (Ctrl+D to save).
```

## 📄 File & Directory Operations

```bash
touch filename    # Create an empty file.
mkdir foldername  # Create a new directory.
mkdir -p path     # Create nested directories.

rmdir foldername  # Remove an empty directory.
```

## 🖥️ Terminal & User Info

```bash
tty               # Shows current terminal device.
whoami            # Current logged-in user.
who               # List logged-in users.
```

## 🖥️ System Info

```bash
uname             # Kernel name.
uname -a          # Full system/kernel info.
```

## 🧠 Hardware Info

```bash
lscpu             # CPU details.
lsusb             # USB devices info.
lspci             # PCI devices info.
lsblk             # Block devices info.
free -h           # Memory usage (human-readable).
dmidecode --type 17  # RAM slot details (requires root).
```

## 📆 Date & Calendar

```bash
date              # Show current date and time.
cal               # Display calendar.
```

## 🌐 Network Info

```bash
ifconfig          # Show network interfaces (deprecated).
ifconfig -a       # Show all interfaces.
ip                # Modern replacement for ifconfig.
ip -a             # Show all interfaces.
ip address        # Show assigned IP addresses.
```

## 📊 File Line Count

```bash
cat n.txt | wc -l  # Count lines in 'n.txt'.
```

## 🗣️ Echo & Shell Info

```bash
echo nikhilpatidar01      # Print a message.
echo $SHELL               # Current shell.
```

## 🕘 Command History

```bash
history                  # Show command history.
history | less           # View history with paging.
history 10               # Show last 10 commands.
history -c               # Clear command history.
```

## 👤 User Information

```bash
id                       # Show user UID, GID, and group info.
```

## 📡 Routing Table

```bash
route -n                 # Show routing table with gateway/interface info.
```

## ⚙️ System Management

```bash
shutdown -P             # Power off the system.
shutdown -P now         # Power off immediately.
shutdown -c             # Cancel shutdown.
shutdown -h             # Halt the system.
shutdown -r             # Reboot the system.
```

## ⏱️ System Uptime

```bash
uptime                  # Show how long the system has been running.
```

## 🧹 Miscellaneous

```bash
clear                   # Clear terminal screen.
exit                    # Exit terminal session.
```


---

# 🧹 File & Directory Deletion Commands

### 🗂️ Remove Directories

```bash
rmdir -p d7/d1/d2/d3/d4/d5     # Remove nested empty directories.
rmdir nikhil*                  # Remove empty directories matching pattern.
rmdir *.txt                    # Tries to remove directories with `.txt` extension (if any).
```

### 🗑️ Remove Files

```bash
rm n.txt                      # Delete a single file.
rm -r n.txt                   # Delete file/directory recursively.
rm -rv n.txt                  # Recursive delete with verbose output.
rm -rfv d4                    # Force delete 'd4' directory with verbose output.
rm -rfv nikhil.txt Patidar.txt lastlog maillog  # Delete multiple files forcefully and verbosely.
rm -rfv *                     # Delete everything in current directory (careful!).
rm -frv b*                    # Delete all items starting with 'b'.
rm -fv *.log                  # Delete all `.log` files with verbose output.
rm -fv hel??                  # Delete files matching 'hel' + any 2 characters.
```

---

# 📋 Copying Files & Directories

```bash
cp -v /etc/passwd /etc/group /etc/gshadow root/desktop/d1/  # Copy with verbose output.
cp -v /var/log/* root/desktop                                # Copy all logs.
cp -vr /var/log/* .                                          # Copy directory recursively and verbosely.
```

---

# 🔀 Moving & Renaming Files

### 🪢 Move (mv command)

```bash
mv file.txt /home/user/Documents/             # Move file to destination.
mv old_name.txt new_name.txt                 # Rename file.
mv -i old_file.txt /home/user/Documents/     # Prompt before overwrite.
mv -f old_file.txt /home/user/Documents/     # Force move (no prompt).
mv -u file.txt /home/user/Documents/         # Move only if newer.
```

---

# 🆕 Rename Files

### ✏️ Rename Using `cp` (Copy and Rename)

```bash
cp Nikhilpatidar1 nikhilpatidar2
```

### ✏️ Rename Using `rename`

```bash
rename 's/.txt$/.bak/' *.txt       # Rename .txt to .bak.
rename 'y/A-Z/a-z/' *              # Convert filenames to lowercase.
rename 's/^/old_/' *.jpg           # Add prefix "old_" to JPG files.
rename 's/ /_/g' *                 # Replace spaces with underscores.
```

### ✏️ Rename Using `mmv`

```bash
mmv "*.txt" "#1_backup.txt"        # Rename all `.txt` to `_backup.txt`.
```

---

# 🔠 Wildcard (Globbing) Patterns

| Pattern | Example           | Matches                        |
| ------- | ----------------- | ------------------------------ |
| `*`     | `*.txt`           | All `.txt` files               |
| `?`     | `file?.txt`       | file1.txt, fileA.txt           |
| `[]`    | `file[123].txt`   | file1.txt, file2.txt           |
| `{}`    | `file{1,2,3}.txt` | file1.txt, file2.txt           |
| `-`     | `file[a-d].txt`   | filea.txt to filed.txt         |
| `!`     | `file[!a-c].txt`  | filed.txt, filee.txt (not a-c) |
| `**`    | `**/*.txt`        | All `.txt` files recursively   |

### Examples:

```bash
rm -fv ma*                         # Remove files starting with 'ma'.
rm -f message?                     # Remove files like message1, messageA.
rm -vf message[2-5]                # Remove message2, message3, message4, message5.
rm -vf message[1,3,5]              # Remove message1, message3, message5.
rm -vf message[!1]                 # Remove all message files except message1.
```

---

# 🔗 Pipes & Redirection

### 📊 Common Pipe Usage

```bash
ls | wc -l                         # Count number of items in current directory.
ls > filelist.txt                 # Save output to file (overwrite).
ls >> filelist.txt                # Append output to file.
wc -l < filelist.txt              # Count lines in file.
ps aux | grep firefox             # Search firefox process.
```

### 📤 Redirection Operators

```bash
ls /nonexistentfolder 2> error.log        # Redirect error to file.
ls /invalidfolder 2>> error.log           # Append error to error file.
ls /home /invalidfolder &> output.log     # Redirect both stdout and stderr.
ls | tee filelist.txt                     # Save output to file and display.
ls | grep ".txt" > textfiles.txt          # Filter `.txt` files into a file.
cat names.txt | sort > sorted_names.txt   # Sort file and save.
```

---

# 📦 Compression Tools

### 🗜️ Gzip

```bash
gzip filename
gzip file1 file2 file3
gunzip filename.gz
gzip -l filename.gz       # Show compression details.
```

### 🗜️ Bzip2

```bash
bzip2 filename
bzip2 file1 file2 file3
bunzip2 filename.bz2
```

### 🗜️ XZ Compression

```bash
xz filename
xz file1 file2 file3
unxz filename.xz
```

---

# 📁 Archiving Files

### 📦 TAR (Tape Archive)

```bash
tar -cvf archive.tar.gz nikhil.txt Patidar.txt   # Create tarball (uncompressed).
tar -xvf archive.tar.gz                          # Extract tar archive.
```

### 🧳 ZIP

```bash
zip archive.zip filename
zip archive.zip file1 file2 file3
unzip archive.zip
```

### 💼 7-Zip

```bash
7z a archive.7z filename      # Add to archive.
7z x archive.7z               # Extract archive.
```

---


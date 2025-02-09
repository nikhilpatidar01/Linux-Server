It seems like you're trying to run a series of `rpm` (Red Hat Package Manager) commands to query and check for packages installed on your system. However, the commands you provided have some syntax issues, and I'll correct them for you.

Here’s a list of the corrected commands with explanations:

### **1. Basic RPM Queries:**
- **`rpm -q firefox`**  
  This will query if the `firefox` package is installed.
  
- **`rpm -q nmap`**  
  This will query if the `nmap` package is installed.

- **`rpm -q apache`**  
  This will query if a package related to `apache` is installed.

- **`rpm -q httpd`**  
  This will query if the `httpd` package (Apache HTTP Server) is installed.

- **`rpm -q ftp`**  
  This will query if any FTP-related package is installed.

- **`rpm -q vsftpd`**  
  This will query if the `vsftpd` (Very Secure FTP Daemon) package is installed.

- **`rpm -q vim`**  
  This will query if the `vim` package is installed.

- **`rpm -q nano`**  
  This will query if the `nano` package is installed.

### **2. List All Installed Packages:**
- **`rpm -qa`**  
  This will list all the packages installed on your system.

### **3. Find Packages Using `grep`:**
- **`rpm -qa | grep vim`**  
  This will list all installed packages and filter out those that contain "vim" in their name.

- **`rpm -qa | grep vim-enhanced`**  
  This will filter out the `vim-enhanced` package from the installed packages list.

- **`rpm -qa | grep nmap`**  
  This will search for `nmap` among installed packages.

- **`rpm -qa | grep openssh`**  
  This will search for OpenSSH-related packages among the installed ones.

### **4. Get Information About a Specific Package:**
- **`rpm -qi openssh-server`**  
  This will display detailed information about the `openssh-server` package.

- **`rpm -ql openssh-server`**  
  This will list all the files installed by the `openssh-server` package.

- **`rpm -qc openssh-server`**  
  This will list the configuration files installed by the `openssh-server` package.

- **`rpm -qd openssh-server`**  
  This will list the documentation files installed by the `openssh-server` package.

- **`rpm -qR openssh-server`**  
  This will list the dependencies required by the `openssh-server` package.

### **5. Querying Packages with Specific Information:**
- **`rpm -qa --last`**  
  This will show the installed packages sorted by their installation date (most recent first).

- **`rpm -qa | wc -l`**  
  This will count the total number of installed packages by piping the output of `rpm -qa` to `wc -l`.

---

### Common Command Syntax Issues in Your Input:
- For `grep` to work properly, it must be combined with `|` to pipe the output of `rpm -qa`.
- `rpm -qa` should be followed by proper options like `--last` or `| grep` for filtering.
- You had a couple of typos like `rpm server` and `rpm -qRopenssh-`, which should be `rpm -qR openssh-server` or similar commands depending on the package you're querying.

---

### Correct Examples:
1. **List all installed packages:**
   ```bash
   rpm -qa
   ```

2. **Filter the installed packages by "vim":**
   ```bash
   rpm -qa | grep vim
   ```

3. **Check the detailed info of `openssh-server`:**
   ```bash
   rpm -qi openssh-server
   ```

4. **Count the total number of installed packages:**
   ```bash
   rpm -qa | wc -l
   ```

5. **List installed packages sorted by installation date:**
   ```bash
   rpm -qa --last
   ```

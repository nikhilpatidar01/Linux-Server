
# ✅ Install and Configure PostgreSQL with LDAP (Debian/Kali/Ubuntu)

### 🔧 Step 1: Install PostgreSQL

```bash
apt update
apt install postgresql postgresql-contrib
```

### 🔍 Step 2: Check PostgreSQL Service

```bash
systemctl status postgresql
```

### 🛠️ Step 3: Configure LDAP Authentication

Edit `pg_hba.conf`:

```bash
vim /etc/postgresql/*/main/pg_hba.conf
```

Add this line at the bottom:

```
host    all             all             0.0.0.0/0               ldap ldapserver=192.168.169.130 ldapbasedn="ou=People,dc=server,dc=local"
```

### 🛠️ Step 4: Allow External Connections

Edit `postgresql.conf`:

```bash
vim /etc/postgresql/*/main/postgresql.conf
```

Uncomment and set:

```
listen_addresses = '*'
```

### 🔄 Step 5: Restart and Verify Service

```bash
systemctl restart postgresql
systemctl status postgresql
```

### 🔗 Step 6: Connect to PostgreSQL via LDAP

```bash
psql -h 192.168.169.130 -U nikhil -d postgres
```

---

### 📚 PostgreSQL Commands Cheat Sheet

#### 💾 Database Management

| Command                 | काम (Description)             |
| ----------------------- | ----------------------------- |
| `\l`                    | सभी databases की सूची दिखाएं  |
| `\c dbname`             | किसी database से connect करें |
| `CREATE DATABASE mydb;` | नया database बनाएं            |
| `DROP DATABASE mydb;`   | database को delete करें       |

#### 👤 User Management

| Command                                   | काम (Description)              |
| ----------------------------------------- | ------------------------------ |
| `\du`                                     | सभी users/roles की सूची दिखाएं |
| `CREATE USER user1 WITH PASSWORD 'pass';` | नया user बनाएं                 |
| `DROP USER user1;`                        | user को delete करें            |
| `ALTER USER user1 WITH SUPERUSER;`        | किसी user को superuser बनाएं   |

#### 📋 Table Management

| Command                                                     | काम (Description)         |
| ----------------------------------------------------------- | ------------------------- |
| `\dt`                                                       | सभी tables की सूची दिखाएं |
| `CREATE TABLE students (id SERIAL PRIMARY KEY, name TEXT);` | नई table बनाएं            |
| `DROP TABLE students;`                                      | table को delete करें      |
| `\d students`                                               | table की structure दिखाएं |

---

# ✅ Joomla Installation with HTTPS (Port 443)

### 🔧 Step 1: Install Required Packages

```bash
apt update
apt install apache2 php php-mysql php-ldap php-xml php-mbstring php-zip php-curl libapache2-mod-php mariadb-server unzip
apt install php-pgsql
```

### 🔁 Step 2: Restart Apache

```bash
systemctl restart apache2
```

### 📁 Step 3: Download and Extract Joomla

```bash
cd /var/www/html
mkdir joomla
cd joomla
wget https://downloads.joomla.org/cms/joomla5/5-3-1/Joomla_5-3-1-Stable-Full_Package.zip
unzip Joomla_5-3-1-Stable-Full_Package.zip
chown -R www-data:www-data /var/www/html/joomla
```

### 🌐 Step 4: Access Joomla Web Installer

Visit:
`http://192.168.169.130/joomla`
or after SSL setup:
`https://192.168.169.130/joomla`

---

# 🔐 Enable HTTPS for Joomla

### 🗝️ Step 5: Create SSL Certificate

```bash
mkdir -p /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/apache2/ssl/joomla.key \
  -out /etc/apache2/ssl/joomla.crt
```

### 📝 Step 6: Configure Virtual Host

```bash
vim /etc/apache2/sites-available/joomla.conf
```

Paste the following:

```apache
<VirtualHost *:443>
    ServerName ns.server.local
    ServerAlias 192.168.169.130
    DocumentRoot /var/www/html

    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/joomla.crt
    SSLCertificateKeyFile /etc/apache2/ssl/joomla.key

    <Directory /var/www/html/>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/joomla_error.log
    CustomLog ${APACHE_LOG_DIR}/joomla_access.log combined
</VirtualHost>
```

### 🖥️ Step 7: Update Hosts File

#### On Server:

```bash
echo "192.168.169.130 ns.server.local" >> /etc/hosts
```

#### On Client (Windows):

Edit `C:\Windows\System32\drivers\etc\hosts`:

```
192.168.169.130 ns.server.local
```

---

### 🔄 Step 8: Enable SSL and Virtual Host

```bash
a2enmod ssl
a2enmod rewrite
a2ensite joomla.conf
systemctl reload apache2
```

### 🌐 Final Joomla Access

* `https://192.168.169.130/joomla`
* `https://ns.server.local/joomla`

---


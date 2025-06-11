
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
```
```
mkdir joomla
```
```
cd joomla
```
```
wget https://downloads.joomla.org/cms/joomla5/5-3-1/Joomla_5-3-1-Stable-Full_Package.zip
```
```
unzip Joomla_5-3-1-Stable-Full_Package.zip
```
```
chown -R www-data:www-data /var/www/html/joomla
```

### 🌐 Step 4: Access Joomla Web Installer

Visit:
```
`http://192.168.169.130/joomla`
```
or after SSL setup:
```
`https://192.168.169.130/joomla`
```
---

# 🔐 Enable HTTPS for Joomla

### 🗝️ Step 5: Create SSL Certificate

```bash
mkdir -p /etc/apache2/ssl
```
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/joomla.key -out /etc/apache2/ssl/joomla.crt
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


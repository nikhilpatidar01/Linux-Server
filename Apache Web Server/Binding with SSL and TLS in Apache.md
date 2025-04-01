# Binding with SSL/TLS in Apache

## 1. Install and Configure SSL/TLS in Apache

### Step 1: Install `mod_ssl`
`mod_ssl` is an Apache module that provides SSL and TLS support.

```bash
yum install mod_ssl
```

### Step 2: Verify `mod_ssl` Installation
Check if the module is installed:

```bash
rpm -qi mod_ssl
rpm -ql mod_ssl
rpm -qd mod_ssl
```

### Step 3: Configure SSL in Apache
Edit the SSL configuration file:

```bash
nano /etc/httpd/conf.d/ssl.conf
```
Ensure that SSL is enabled by setting appropriate certificate files:

```apache
SSLCertificateFile /etc/pki/tls/certs/localhost.crt
SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
```

### Step 4: Restart Apache
Restart Apache to apply SSL configuration:

```bash
systemctl restart httpd.service
```

### Step 5: Verify Apache Listening on SSL Port
Check if Apache is listening on port 443 (SSL):

```bash
netstat -nltup | grep httpd
```

### Step 6: Update Firewall for SSL
Edit the firewall rules to allow SSL traffic (port 443):

```bash
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload
```

---

## 2. Create Self-Signed SSL Certificates

### Step 1: Create SSL Directory
Create a directory to store SSL certificates:

```bash
mkdir /opt/ssl
cd /opt/ssl
```

### Step 2: Generate SSL Certificate and Key
Generate a self-signed SSL certificate and key:

```bash
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365
```

### Step 3: Check the Generated Files
Verify the files:

```bash
ls -lh
file key.pem
file cert.pem
```

### Step 4: Copy SSL Files to Appropriate Directories
Copy the certificate and key to the proper directories:

```bash
cp -v /opt/ssl/cert.pem /etc/pki/tls/certs/cert.pem
cp -v /opt/ssl/key.pem /etc/pki/tls/private/key.pem
```

---

## 3. Update Apache Configuration with SSL Certificates
Edit the Apache configuration to point to the newly created certificates:

```bash
nano /etc/httpd/conf.d/ssl.conf
```
Modify or add these lines:

```apache
SSLCertificateFile /etc/pki/tls/certs/cert.pem
SSLCertificateKeyFile /etc/pki/tls/private/key.pem
```

Restart Apache again:

```bash
systemctl restart httpd.service
```

---

## 4. Verify SSL Binding in Apache
Check if Apache is listening on both HTTP (port 80) and HTTPS (port 443):

```bash
netstat -nltup | grep httpd
```

---

## 5. Update Firewall for SSL/TLS Ports
Ensure that port 443 is open in your firewall configuration:

```bash
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload
```

---

## 6. Configure Apache Virtual Hosts with SSL/TLS
You can create Virtual Hosts with SSL/TLS enabled.

### Open Configuration File

```bash
nano /etc/httpd/conf/httpd.conf
```

### Example of Apache Virtual Hosts for HTTP and HTTPS

```apache
<VirtualHost *:80>
    ServerName site1.local
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost *:443>
    ServerName site1.local
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/cert.pem
    SSLCertificateKeyFile /etc/pki/tls/private/key.pem
</VirtualHost>

<VirtualHost *:80>
    ServerName armour.local
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost *:443>
    ServerName armour.local
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/cert.pem
    SSLCertificateKeyFile /etc/pki/tls/private/key.pem
</VirtualHost>
```

---

## 7. Advanced Configuration with Custom CA (Certificate Authority)
If you want to use a self-generated Certificate Authority (CA):

### Step 1: Generate the Private Key for the CA

```bash
openssl genrsa -out ca.key 2048
```

### Step 2: Generate the Certificate Signing Request (CSR)

```bash
openssl req -new -key ca.key -out ca.csr
```

### Step 3: Create the Self-Signed Certificate for the CA

```bash
openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt
```

### Step 4: Copy the CA Certificate and Key to the Appropriate Directories

```bash
cp -v /opt/ssl/ca.crt /etc/pki/tls/certs/ca.crt
cp -v /opt/ssl/ca.key /etc/pki/tls/private/ca.key
```

---

## 8. Final Steps
After configuring your Virtual Hosts and SSL certificates, restart Apache and check the ports once again:

```bash
systemctl restart httpd.service
netstat -nltup | grep httpd
```

Test your websites using both HTTP and HTTPS URLs:

| Domain             | Port | Expected Output                 |
|-------------------|------|--------------------------------|
| http://nikhil.local | 80   | Site1 homepage                 |
| https://nikhil.local | 443  | Site1 (SSL-enabled) homepage  |

---


# Binding with Port Number in Apache

Apache can bind to specific ports, allowing you to run multiple websites on different ports using Virtual Hosts. This is useful when you want to test different sites on the same IP but using unique ports.

## 1. Modify Apache Configuration

Open the main Apache configuration file:
```bash
vim /etc/httpd/conf/httpd.conf
```

### Add Listen Directives for New Ports
The Listen directive tells Apache which ports to bind to. Add the following lines to allow Apache to listen on ports 80, 8080, 8081, and 8082:

```apache
Listen 80
Listen 8080
Listen 8081
Listen 8082
```

Save and exit.

### Open Ports in Firewall
```bash
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --permanent --add-port=8081/tcp
firewall-cmd --permanent --add-port=8082/tcp
firewall-cmd --reload
```

## 2. Create Virtual Hosts in a Single File

Open the main Apache configuration file:
```bash
nano /etc/httpd/conf/httpd.conf
```
Ensure the following line is present to allow virtual hosts *(should be at the last line)*:
```apache
IncludeOptional conf.d/*.conf
```

Add multiple Virtual Host entries:
```apache
<VirtualHost 192.168.112.145:8080>
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.112.145:8081>
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.112.145:8082>
    DocumentRoot /var/www/html/site3/
    DirectoryIndex index.html
</VirtualHost>
```

Apply the changes by restarting Apache:
```bash
systemctl restart httpd.service
```

## 3. Create Virtual Hosts as Separate Files

Create separate Virtual Host files for each port under `/etc/httpd/conf.d/`.

### 3.1. Create a Virtual Host for Port *80*:
#### site1.conf
```bash
nano /etc/httpd/conf.d/site1.conf
```
Example:
```apache
<VirtualHost 192.168.112.145:80>
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
</VirtualHost>
```

### 3.2. Create a Virtual Host for Port *8080*:
#### site2.conf
```bash
nano /etc/httpd/conf.d/site2.conf
```
Example:
```apache
<VirtualHost 192.168.112.145:8080>
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>
```

### 3.3. Create a Virtual Host for Port *8081*:
#### site3.conf
```bash
nano /etc/httpd/conf.d/site3.conf
```
Example:
```apache
<VirtualHost 192.168.112.145:8081>
    DocumentRoot /var/www/html/site3/
    DirectoryIndex index.html
</VirtualHost>
```

### 3.4. Create a Virtual Host for Port *8082*:
#### site4.conf
```bash
nano /etc/httpd/conf.d/site4.conf
```
Example:
```apache
<VirtualHost 192.168.112.145:8082>
    DocumentRoot /var/www/html/site4/
    DirectoryIndex index.html
</VirtualHost>
```

Apply the changes by restarting Apache:
```bash
systemctl restart httpd.service
```

## 4. Set Permissions
```bash
chown -Rv apache:apache /var/www/html/site1 /var/www/html/site2 /var/www/html/site3 /var/www/html/site4
```

## 5. Validate Configuration
Check if the Apache configuration is valid:
```bash
httpd -t
```
Example output:
```
Syntax OK
```

## 6. Restart Apache
Apply the changes by restarting the Apache service:
```bash
systemctl restart httpd.service
```

## 7. Test the Configuration
Use `netstat` to confirm Apache is listening on the correct ports:
```bash
netstat -nltup | grep httpd
```

## Summary

| Port  | Virtual Host Configuration | Purpose          |
|-------|---------------------------|-----------------|
| 80    | `<VirtualHost 192.168.112.145:80>`    | Main Site 1       |
| 8080  | `<VirtualHost 192.168.112.145:8080>`  | Test site 2     |
| 8081  | `<VirtualHost 192.168.112.145:8081>`  | Test site 3     |
| 8082  | `<VirtualHost 192.168.112.145:8082>`  | Test site 4     |

**Apache virtual host binding with ports is now complete!**

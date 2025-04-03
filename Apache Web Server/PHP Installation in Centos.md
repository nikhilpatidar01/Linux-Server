# PHP Installation and Configuration

## Install Required Repositories
```sh
yum install epel-release yum-utils
yum repolist all
```
Visit the official Remi repository for the latest repository package:
[Remi Repository](https://rpms.remirepo.net/)

```sh
yum install http://rpms.remirepo.net/enterprise/remi-release-9.rpm
yum repolist all
rpm -qc remi-release
rpm -ql remi-release
vim /etc/yum.repos.d/remi.repo
yum repolist all
```

## Search for Available PHP Versions
```sh
yum search php
yum info php.x86_64
yum info php81.x86_64
yum info php84.x86_64
```

## Install PHP and Required Extensions
```sh
yum install php php-common.x86_64 php-cli.x86_64 php-opcache.x86_64 php-gd.x86_64 php-curl php-mysqlnd.x86_64 php-xml.x86_64 php-mbstring.x86_64 php-pear php-mbstring php-pecl-http php-session
```

## Verify PHP Installation
```sh
php -v
```

## Restart Apache
```sh
systemctl restart httpd.service
```

## Create phpinfo.php for Testing
```sh
nano /var/www/html/phpinfo.php
```
Add the following content:
```php
<?php
    phpinfo();
?>
```
Access `phpinfo.php` via:
```
http://192.168.112.145/phpinfo.php
```

---

# MySQL Installation and Configuration

## Download and Install MySQL Repository
```sh
wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
yum install ./mysql80-community-release-el9-1.noarch.rpm
yum repolist all
```

## Enable and Disable MySQL Versions
```sh
yum-config-manager --enable mysql80-community
yum-config-manager --enable mysql57-community
yum-config-manager --disable mysql57-community
```

## Search for MySQL Packages
```sh
yum search mysql
```

## Install MySQL Server and Development Packages
```sh
yum install mysql-community-server mysql-community-devel
```

## Check MySQL Service Status
```sh
systemctl status mysqld.service
systemctl enable mysqld.service
netstat -nltup | grep mysql
```

## Start and Restart MySQL Service
```sh
systemctl start mysqld.service
systemctl restart mysqld.service
netstat -nltup | grep mysql
```

## Retrieve MySQL Root Password
```sh
ls -lh /var/log/mysqld.log
grep 'temporary password' /var/log/mysqld.log
```
Example Output:
```
A temporary password is generated for root@localhost: s;r=oAt25UuR
```

## Secure MySQL Installation
```sh
mysql -u root -p
```
Enter the temporary password when prompted.
```sh
mysql_secure_installation
```
Example new password:
```
p@ssWord@1234
```

## Login with New MySQL Root Password
```sh
mysql -u root -p p@ssWord@1234
```

## Change Root Password (If Needed)
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Nikhil@123';
SHOW DATABASES;
EXIT;
```

---

# phpMyAdmin Installation and Configuration

Visit the official phpMyAdmin website:
[phpMyAdmin](https://www.phpmyadmin.net/)

## Download and Extract phpMyAdmin
```sh
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.2/phpMyAdmin-5.2.2-all-languages.zip
unzip phpMyAdmin-5.2.2-all-languages.zip
mkdir /var/www/html/phpmyadmin
cp -vr phpMyAdmin-5.2.2-all-languages/* /var/www/html/phpmyadmin/
```

## Configure phpMyAdmin
```sh
cd /var/www/html/
cp /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/config.inc.php
vim /var/www/html/phpmyadmin/config.inc.php
```
Generate a random secret passphrase:
```sh
pwgen 32 -1
```

## Set Proper Ownership
```sh
chown -Rv apache:apache /var/www/html/phpmyadmin
```

## Restart Apache
```sh
systemctl restart httpd.service
```

---

# Access phpMyAdmin Admin Panel

## Open phpMyAdmin in Browser
```
http://192.168.112.145/phpmyadmin
```

## Login Credentials
- **Username:** root
- **Password:** (your MySQL root password)

## Troubleshooting
1. If phpMyAdmin displays errors related to `mbstring` or `openssl`, ensure PHP extensions are installed:
   ```sh
   yum install php-mbstring php-openssl -y
   systemctl restart httpd.service
   ```
2. If access is denied, update MySQL permissions:
   ```sql
   GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'YourPassword' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   ```


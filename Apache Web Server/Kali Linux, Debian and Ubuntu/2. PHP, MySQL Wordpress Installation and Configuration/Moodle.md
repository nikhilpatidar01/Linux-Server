

# 📘 Moodle Installation Guide (With HTTPS on Port 443)

---

## 🅰️ Step 1: Install Required Packages

```bash
apt update
apt install apache2 mariadb-server php php-mysql libapache2-mod-php php-xml php-gd php-curl php-intl php-zip php-soap php-mbstring php-bcmath php-cli git unzip wget -y
```

---

## 🅱️ Step 2: Create Moodle Database

```bash
mysql -u moodleuser -p
```

When prompted, enter your MySQL root password. Example:

```
Enter password: Nikhil@123
```

Inside MySQL, create a database and user:

```sql
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'StrongPassword@123';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## 🆎 Step 3: Download and Set Up Moodle

```bash
cd /var/www/html
wget https://download.moodle.org/download.php/direct/stable500/moodle-latest-500.zip -O moodle-5.0.zip
unzip moodle-5.0.zip
mv moodle /var/www/html/moodle
chown -R www-data:www-data /var/www/html/moodle
chmod -R 755 /var/www/html/moodle
```

### Create Moodle Data Directory:

```bash
mkdir /var/www/moodledata
chown -R www-data:www-data /var/www/moodledata
chmod -R 770 /var/www/moodledata
```

---

## 🔐 Step 4: Configure SSL (Self-Signed or Custom Certificate)

If using **self-signed certificate**, generate using:

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/ssl/private/your_cert.key \
-out /etc/ssl/certs/your_cert.crt
```

Follow prompts and fill required info.

---

## 🌐 Step 5: Configure Apache Virtual Host for Moodle (HTTPS)

Create new config file:

```bash
nano /etc/apache2/sites-available/moodle.conf
```

Add the following content:

```apache
<VirtualHost *:443>
    ServerAdmin admin@yourdomain.com
    ServerName moodle.yourdomain.com

    DocumentRoot /var/www/html/moodle

    <Directory /var/www/html/moodle>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Require all granted
    </Directory>

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/your_cert.crt
    SSLCertificateKeyFile /etc/ssl/private/your_cert.key
</VirtualHost>
```

---

## 🔄 Step 6: Enable SSL and Site

```bash
a2enmod ssl
a2ensite moodle.conf
a2dissite 000-default.conf
systemctl reload apache2
```

---

## ✅ Final Step: Complete Moodle Installation via Browser

Open your browser and visit:

```
https://192.168.180.53/moodle
```


---

## 🧭 Moodle Web Installer — Step-by-Step Guide

URL to open in browser:
**[https://192.168.180.53/moodle/install.php](https://192.168.180.53/moodle/install.php)**

---

### ✅ Step 1: Select Language

* Choose your **preferred language** (e.g. English).
* Click **Next**.

---

### ✅ Step 2: Confirm Paths

* Moodle will auto-detect:

  * **Web address**: `https://192.168.180.53/moodle`
  * **Moodle directory**: `/var/www/html/moodle`
  * **Data directory**: `/var/www/moodledata`

✔ If everything is correct, click **Next**.

---

### ✅ Step 3: Choose Database Driver

Select database type:

* Choose **MariaDB / MySQL** (Improved native mysqli driver)

Click **Next**.

---

### ✅ Step 4: Configure Database Settings

Fill the following:

| Field                 | Value                        |
| --------------------- | ---------------------------- |
| **Database host**     | `localhost`                  |
| **Database name**     | `moodle`                     |
| **Database user**     | `moodleuser`                 |
| **Database password** | `StrongPassword@123`         |
| **Tables prefix**     | `mdl_` (default, keep as is) |

Then click **Next**.

---

### ✅ Step 5: Server Check

* Moodle will now check your PHP extensions, permissions, etc.
* Make sure everything is green.
* If anything is **red or missing**, go back and install required packages.

Click **Continue** when ready.

---

### ✅ Step 6: License Agreement

* Read and accept the **GPL license agreement**.
* Click **Continue**.

---

### ✅ Step 7: Installation Progress

* Moodle will now install tables into the database.
* This process can take a few minutes.
* Wait until it finishes and click **Continue**.

---

### ✅ Step 8: Configure Site Settings

Fill your Moodle site information:

| Field                  | Example                        |
| ---------------------- | ------------------------------ |
| **Full site name**     | `Nikhil's LMS Platform`        |
| **Short name**         | `NIKLMS`                       |
| **Front page summary** | `E-learning portal by Nikhil`  |
| **Admin username**     | `admin`                        |
| **Admin password**     | `StrongAdmin@123`              |
| **Admin email**        | `admin@example.com`            |
| **Timezone**           | Set according to your location |

Click **Update profile** or **Save**.

---

### ✅ Step 9: Front Page Settings

You can configure:

* Course formats
* Default role
* Display settings

You can skip or configure later.

Click **Save changes**.

---

### ✅ Step 10: Moodle Dashboard

You are now on the Moodle **Admin Dashboard** 🎉

From here, you can:

* Create courses
* Add users
* Change themes
* Configure plugins

---

## ✅ Done!

Your Moodle is now successfully installed and live at:

```
https://192.168.180.53/moodle
```

---





# How to Configure Nginx on CentOS/RHEL

This guide explains how to install, configure, and secure an Nginx web server on CentOS or RHEL systems.

---

## 1. Update System Packages

Before installing anything, ensure your system is up to date:

```bash
sudo yum update -y
```

---

## 2. Install Nginx

Enable the EPEL repository and install Nginx:

```bash
sudo yum install epel-release -y
sudo yum install nginx -y
```

### Check Nginx Status:

```bash
sudo systemctl status nginx
```

If it’s not running, start and enable it:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## 3. Configure Firewall (firewalld)

Allow Nginx HTTP and HTTPS traffic through the firewall:

```bash
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```

### Check Firewall Status:

```bash
sudo firewall-cmd --list-all
```

---

## 4. Basic Nginx Configuration

Edit the default server block:

```bash
sudo vi /etc/nginx/nginx.conf
```

Alternatively, you can create a custom config file in `/etc/nginx/conf.d/`.

### Sample Server Block (`/etc/nginx/conf.d/default.conf`):

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /usr/share/nginx/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Save and exit (`ESC`, `:wq`).

---

## 5. Test and Reload Nginx

### Test Configuration:

```bash
sudo nginx -t
```

### Reload Nginx:

```bash
sudo systemctl reload nginx
```

---

## 6. Place Your Website Files

Upload your website content into:

```
/usr/share/nginx/html/
```

### Example: Create a Sample `index.html`

```bash
sudo vi /usr/share/nginx/html/index.html
```

Paste this HTML:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Website</title>
</head>
<body>
    <h1>Success! Nginx is working!</h1>
</body>
</html>
```

Save and exit.

---

## 7. Optional: Enable HTTPS with Let’s Encrypt (Certbot)

Install Certbot and the Nginx plugin:

```bash
sudo yum install certbot python3-certbot-nginx -y
```

### Get and Install SSL Certificate:

```bash
sudo certbot --nginx
```

Follow the on-screen prompts to configure HTTPS.

---

## Done

Your website should now be live at:

```
http://yourdomain.com
```

or (if SSL is configured):

```
https://yourdomain.com
```

---

## Example Folder Structure

```
/etc/nginx/nginx.conf
/etc/nginx/conf.d/default.conf
/usr/share/nginx/html/index.html
```

---

Let me know if you’d like versions for:
- Hosting multiple sites (server blocks)
- Reverse proxying to Node.js, PHP, etc.
- Load balancing multiple backend servers

# Server Installation and Webuzo Setup Guide

---

## 1. Update Network Configuration

Edit the netplan YAML file:

```bash
sudo nano /etc/netplan/*.yaml
```

Add or modify with your static IP and DNS settings:

```yaml
network:
  version: 2
  ethernets:
    eno2:
      addresses:
        - 192.168.50.100/24
      routes:
        - to: default
          via: 192.168.50.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```

Apply the configuration:

```bash
sudo netplan apply
```

---

## 2. Update and Upgrade Ubuntu

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 3. Download and Install Webuzo

Download the installer script:

```bash
wget -N http://files.webuzo.com/install.sh
```

Make it executable and run:

```bash
chmod 755 install.sh
sudo ./install.sh
```

---

## 4. (Optional) Configure Firewall

Open required Webuzo ports:

```bash
sudo ufw allow 2004/tcp
sudo ufw allow 2005/tcp
sudo ufw reload
```

---

## 5. Configure Apache DocumentRoot for Your Domain

Edit Webuzo’s Apache virtual host config:

```bash
sudo nano /usr/local/apps/apache2/etc/conf.d/webuzoVH.conf
```

Make sure your domain block looks like this (replace `your-domain.com` accordingly):

```apache
<VirtualHost *:80>
    ServerName your-domain.com
    DocumentRoot /home/amarfuel/public_html

    <Directory "/home/amarfuel/public_html">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.(php|phtml)$>
        SetHandler "proxy:unix:/usr/local/apps/php82/var/fpm-amarfuel.sock|fcgi://localhost"
    </FilesMatch>
</VirtualHost>
```

---

## 6. Set File and Directory Permissions

```bash
sudo chown -R amarfuel:amarfuel /home/amarfuel/public_html
sudo chmod -R 755 /home/amarfuel/public_html
```

---

## 7. Restart Apache and Ensure PHP-FPM Is Running

Restart Apache:

```bash
sudo /usr/local/apps/apache2/bin/httpd -k restart
```

Check PHP-FPM processes:

```bash
ps aux | grep php-fpm
```

---

## 8. Troubleshooting PHP-FPM if PHP Does Not Work

### Step 1: Start PHP-FPM Manually

```bash
sudo /usr/local/apps/php82/sbin/php-fpm -y /usr/local/apps/php82/etc/php-fpm.conf
```

If you get an error like:

```
Another FPM instance seems to already listen...
```

Find which process holds the socket:

```bash
sudo lsof | grep fpm-amarfuel.sock
```

Kill all PHP-FPM processes:

```bash
sudo pkill -f php-fpm
```

Start PHP-FPM again:

```bash
sudo /usr/local/apps/php82/sbin/php-fpm -y /usr/local/apps/php82/etc/php-fpm.conf
```

### Step 2: Confirm PHP-FPM is Listening

Check listening ports or socket:

```bash
sudo ss -ltnp | grep 9000
```

Or

```bash
ls -l /usr/local/apps/php82/var/fpm-amarfuel.sock
```

### Step 3: Restart Apache

```bash
sudo /usr/local/apps/apache2/bin/httpd -k restart
```

---

## 9. Fix Stale PHP-FPM Socket Issue

If PHP still fails, do:

### Delete stale socket

```bash
sudo rm -f /usr/local/apps/php82/var/fpm-amarfuel.sock
```

### Start PHP-FPM again

```bash
sudo /usr/local/apps/php82/sbin/php-fpm -y /usr/local/apps/php82/etc/php-fpm.conf
```

### Verify it’s running

```bash
sudo ss -pl | grep fpm
```

or

```bash
sudo lsof | grep fpm-amarfuel.sock
```

### Restart Apache

```bash
sudo /usr/local/apps/apache2/bin/httpd -k restart
```

---

## 10. Verify Apache PHP-FPM Configuration

Check that your Apache VirtualHost includes this PHP handler:

```apache
<FilesMatch \.(php|phtml)$>
    <If "-f %{REQUEST_FILENAME}">
        SetHandler "proxy:unix:/usr/local/apps/php82/var/fpm-amarfuel.sock|fcgi://localhost"
    </If>
</FilesMatch>
```

Restart Apache one last time:

```bash
sudo /usr/local/apps/apache2/bin/httpd -k restart
```

---

## 11. Test PHP Execution

Open in browser:

* [http://your-server-ip/](http://your-server-ip/)
* or [http://your-domain.com/](http://your-domain.com/)

You should see PHP files executing, not downloading.

---

# End of Guide

# 📘 Install phpMyAdmin on Ubuntu & Manage MySQL Credentials

This guide explains how to install **phpMyAdmin** on Ubuntu (without cPanel or Webuzo) and how to check or reset your MySQL username & password.

---

## 1️⃣ Install phpMyAdmin

1. **Update package list**

```bash
sudo apt update
sudo apt upgrade -y
```

2. **Install phpMyAdmin**

```bash
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl -y
```

3. **Enable PHP extensions**

```bash
sudo phpenmod mbstring
sudo systemctl restart apache2
```

4. **Enable phpMyAdmin in Apache**

```bash
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
sudo systemctl reload apache2
```

5. **Access phpMyAdmin**\
   Open in browser:

```
http://<server-ip>/phpmyadmin
```

Example:

```
http://202.5.52.50:8080/phpmyadmin
```

---

## 2️⃣ Find Existing MySQL Users

1. **Log into MySQL as root**

```bash
sudo mysql -u root -p
```

2. **List users**

```sql
SELECT User, Host FROM mysql.user;
```

Example output:

```
+------------------+-----------+
| User             | Host      |
+------------------+-----------+
| root             | localhost |
| phpmyadmin       | localhost |
| amar_fuel_user   | %         |
+------------------+-----------+
```

---

## 3️⃣ Test MySQL Login

Run:

```bash
mysql -u <username> -p
```

- Replace `<username>` with a user from the list.
- Enter the password.

✅ If login works → credentials are correct.\
❌ If login fails → reset the password.

---

## 4️⃣ Reset MySQL Password

1. **Login as root**

```bash
sudo mysql
```

2. **Reset password for a user**

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'NewPassword123!';
FLUSH PRIVILEGES;
```

3. **Test login**

```bash
mysql -u root -p
```

Use your new password.

---

## 5️⃣ phpMyAdmin Login

- Open browser:

```
http://<server-ip>/phpmyadmin
```

- Username: `root` (or another MySQL user)
- Password: The password you set with `ALTER USER`

---

✅ You now have phpMyAdmin installed and can log in with your MySQL username and password.


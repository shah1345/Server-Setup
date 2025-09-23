# Apache SSL Certificate Setup Guide (Certbot)

This guide explains how to secure your Apache server with HTTPS using **Let's Encrypt** free SSL certificates.

---

## 1. Update System Packages
```bash
sudo apt update
sudo apt upgrade -y
```

---

## 2. Install Certbot and Apache Plugin
```bash
sudo apt install certbot python3-certbot-apache -y
```

---

## 3. Run Certbot to Obtain SSL Certificate
```bash
sudo certbot --apache \
  -d server.amarfuel.com \
  -d www.server.amarfuel.com \
  -d binimoy.amarfuel.com \
  -d chystiya.amarfuel.com \
  -d smet.amarfuel.com
```

**Steps During Execution:**
1. **Enter Email Address:**
   - Used for renewal and security notices.
   - Example: `yourname@example.com`

2. **Agree to Terms of Service:**
   - Type `A` or `Y` to agree.

3. **Subscribe to EFF Emails (Optional):**
   - Type `Y` to subscribe, `N` to skip.

4. **Choose HTTP → HTTPS Redirect:**
   - Select `2` to automatically redirect all HTTP traffic to HTTPS (recommended).

---

## 4. Verify SSL Certificate
Open a browser and visit:
```
https://server.amarfuel.com
```
- You should see the **HTTPS lock icon**.

---

## 5. Test Apache Configuration
```bash
sudo apache2ctl configtest
sudo systemctl reload apache2
```
- Ensure it returns `Syntax OK`.

---

## 6. Check Firewall
Make sure port 443 (HTTPS) is allowed:
```bash
sudo ufw status
sudo ufw allow 443/tcp
sudo ufw reload
```

---

## 7. Auto-Renewal of SSL Certificate
Certbot automatically installs a cron job for auto-renewal.
Test renewal with:
```bash
sudo certbot renew --dry-run
```

---

## ✅ Notes
- Each subdomain requires a valid DNS A record pointing to your server IP.
- You can include multiple subdomains in **one Certbot command** using multiple `-d` flags.

---

**References:**
- [Certbot Official Documentation](https://certbot.eff.org/)
- [Apache SSL Configuration](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html)


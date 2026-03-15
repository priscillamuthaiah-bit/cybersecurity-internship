# Internship Project: Web Server Security & Configuration
**Date:** 2026-03-15  
**Service:** Nginx / PHP-FPM  
**Target:** DVWA (Damn Vulnerable Web Application)

---

## 1. Web Server Hardening (Nginx)

### Objective
Configure the Nginx web server to serve the DVWA application over a non-standard port and implement security headers to mitigate common web-based attacks.

### Configuration Details
- **Configuration Path:** `/etc/dvwa/vhost/dvwa-nginx.conf`
- **Listening Port:** `42001`
- **Document Root:** `/usr/share/dvwa/`
- **PHP Socket:** `unix:/var/run/php/php8-fpm-42001.sock`

### Implemented Security Headers
To harden the server, I implemented the following HTTP response headers within the `server` block:
- **X-Frame-Options (DENY):** Prevents Clickjacking attacks by forbidding the page from being rendered in a `<frame>`, `<iframe>`, or `<object>`.
- **X-Content-Type-Options (nosniff):** Prevents the browser from "sniffing" the MIME type, forcing it to stick to the declared content-type and mitigating MIME-type sniffing attacks.
- **Content-Security-Policy (CSP):** Implemented a baseline `default-src 'self'` policy to restrict where resources (scripts, images, etc.) can be loaded from, significantly reducing XSS risks.

---

## 2. Access Control & Validation

### IP Whitelisting
To ensure the lab environment remains private, access was restricted at the Nginx level:
```nginx
location / {
    allow 127.0.0.1;
    deny all;
    try_files $uri $uri/ =404;
}

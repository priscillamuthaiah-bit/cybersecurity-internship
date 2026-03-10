# Penetration Testing Lab: Enumeration & Information Gathering

## 1. Network Topology & Environment
This lab environment consists of a Kali Linux attacker machine and a Metasploitable2 target machine, both hosted on a VirtualBox Host-Only network.

* **Attacker (Kali Linux):** `192.168.56.102`
* **Target (Metasploitable2):** `192.168.56.103`
* **Connectivity Test:** Successful ICMP ping from Kali to target (0% packet loss, avg latency ~3.6ms).

---

## 2. Initial Reconnaissance
Initial connectivity and basic service checks were performed using `ping`, `nc`, and `nslookup`.

| Command | Result | Observation |
| :--- | :--- | :--- |
| `ping 192.168.56.103` | Success | Host is active and reachable. |
| `nc 192.168.56.103 21` | Banner Grab | Confirmed `vsFTPd 2.3.4` is running on port 21. |
| `nslookup 192.168.56.103` | NXDOMAIN | No PTR record found for the target IP. |

---

## 3. Nmap Scanning Phase
Detailed scanning was conducted to identify open ports, service versions, and OS details.

### Stealth Scan (`nmap -sS`)
Identified a massive attack surface with numerous open ports including:
* **Standard Services:** FTP (21), SSH (22), Telnet (23), SMTP (25), DNS (53), HTTP (80).
* **Database/Middleware:** MySQL (3306), PostgreSQL (5432), Tomcat (8180).
* **High-Risk/Legacy:** rpcbind (111), NetBIOS (139/445), VNC (5900), IRC (6667).

### OS & Version Detection (`nmap -sV -O -A`)
* **Operating System:** Linux 2.6.9 - 2.6.33 (likely Ubuntu 8.04).
* **Device Type:** General Purpose (VirtualBox VM).
* **Key Service Versions:**
    * **FTP:** `vsFTPd 2.3.4` (Known backdoor vulnerability).
    * **SSH:** `OpenSSH 4.7p1 Debian 8ubuntu1`.
    * **HTTP:** `Apache httpd 2.2.8 (Ubuntu) DAV/2`.
    * **Samba:** `Samba smbd 3.X - 4.X`.
    * **IRC:** `UnrealIRCd`.

---

## 4. Vulnerability Assessment (Early Findings)

### FTP (Port 21) - vsFTPd 2.3.4
* **Finding:** Anonymous FTP login is allowed (Code 230).
* **Critical Note:** This specific version of vsFTPd is famously known for a "smiley face" backdoor vulnerability (CVE-2011-2523) that allows for instant root shell access.

### SSH (Port 22)
* **Host Keys:** RSA (2048-bit) and DSA (1024-bit).
* **Status:** Running an older version of OpenSSH, potentially susceptible to username enumeration or brute force.

### Bindshell (Port 1524)
* **Finding:** Nmap identified a `Metasploitable root shell` running on this port.
* **Exploitation:** This is a pre-installed backdoor; connecting via Netcat provides immediate root access without authentication.

### SSL/TLS (Port 25/SMTP)
* **Weakness:** Support for SSLv2 identified with legacy ciphers (MD5-based).
* **Certificate Info:** Issued to `ubuntu804-base.localdomain`, expired since April 2010.

---


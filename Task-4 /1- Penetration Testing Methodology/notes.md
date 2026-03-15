# Penetration Testing Milestone: Reconnaissance & Information Gathering
**Target IP:** `192.168.56.103`  
**Attacker IP:** `192.168.56.102`  
**Phase:** Phase 1 (Reconnaissance)

---

## 1. Methodology: The Reconnaissance Process
For this project, I followed a systematic four-step methodology to map the target's attack surface without engaging in active exploitation.

### Step A: Passive Discovery & Connectivity
- **Action:** Verified the target host is "Alive" on the local network.
- **Command:** `ping -c 5 192.168.56.103`
- **Result:** 0% packet loss confirmed a stable connection via VirtualBox network adapters.
### Step B: Footprinting (Whois & DNS)
- **Action:** Attempted to gather registration and domain data.
- **Commands:** `whois 192.168.56.103` | `nslookup 192.168.56.103`
- **Observation:** Resolved to private IANA-reserved range (`RFC 1918`), confirming an isolated lab environment.

### Step C: Active Service Enumeration (Fingerprinting)
- **Action:** Identified open ports and software versions using Nmap.
- **Command:** `nmap -sV -T4 192.168.56.103`
- **Analysis:** Captured banners to identify high-risk legacy services.

### Step D: Vulnerability Mapping
- **Action:** Cross-referencing identified service versions against known exploit databases.
- **Finding:** Identified `vsftpd 2.3.4` on Port 21, which is known for a high-severity backdoor (CVE-2011-2523).
- 

---
## 2. Technical Findings (Port Map)

| Port | Protocol | Service | Version | Potential Risk |
| :--- | :--- | :--- | :--- | :--- |
| 21 | TCP | FTP | vsftpd 2.3.4 | **Backdoor Command Execution** |
| 22 | TCP | SSH | OpenSSH 4.7p1 | Brute-force / Outdated crypto |
| 23 | TCP | Telnet | Linux telnetd | Cleartext credential sniffing |
| 80 | TCP | HTTP | Apache 2.2.8 | Web application vulnerabilities |
| 1524 | TCP | Shell | Bindshell | **Pre-existing root backdoor** |

---

## 3. Toolset Summary
- **Nmap:** Stealthy service discovery and version detection.
- **Ping:** ICMP echo requests to verify host availability.
- **IP Addr:** Mapped local interfaces to ensure proper network routing.

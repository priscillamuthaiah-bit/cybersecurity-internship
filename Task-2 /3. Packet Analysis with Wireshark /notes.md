# Network Traffic Analysis: FTP Session Observation
**Project:** Penetration Testing Lab / Network Forensics  
**Tool:** Wireshark  
**Target:** Metasploitable2 (`192.168.56.103`)  
**Attacker:** Kali Linux (`192.168.56.102`)

---

## 1. Overview
This report documents the packet-level analysis of an unencrypted FTP session. The objective was to observe the handshake, authentication process, and command exchange between the attacker and the target machine.


---

## 2. Packet Analysis Details

### 2.1 Service Identification
The initial response from the server identifies the specific service and version running on port 21.
* **Packet No:** 8
* **Server Banner:** `220 (vsFTPd 2.3.4)`
* **Observation:** The server is running a version of vsFTPd known to contain a critical backdoor vulnerability (CVE-2011-2523).

### 2.2 Unencrypted Authentication
FTP transmits credentials in cleartext, making them easily retrievable via packet sniffing.
* **User Command:** `USER msfadmin` (Packet 12)
* **Password Command:** `PASS msfadmin` (Packet 16)
* **Server Response:** `230 Login successful.` (Packet 17)
* **Risk:** Any actor on the local network could capture these credentials using a tool like Wireshark or tcpdump.

### 2.3 Post-Authentication Activity
After successful login, several standard FTP commands were observed as the attacker navigated the file system:
* **SYST:** Queries the remote system type (`UNIX Type: L8`).
* **PWD:** Prints the current working directory (`"/home/msfadmin"`).
* **LIST:** Requests a directory listing to see available files.
* **TYPE I/A:** Switching between Binary (I) and ASCII (A) transfer modes.

---

## 3. Vulnerability Correlation
The traffic captured aligns with findings from the automated Nessus scan performed on this host.

| Finding Category | Severity | Evidence from Wireshark |
| :--- | :--- | :--- |
| **Cleartext Protocol** | Medium/High | Credentials (`msfadmin:msfadmin`) visible in packets 12 & 16. |
| **Vulnerable Service** | Critical | Banner identifies `vsFTPd 2.3.4` (Packet 8). |
| **Information Leak** | Low | System type and directory structure revealed via `SYST` and `PWD`. |

---

## 4. Remediation & Security Best Practices
1. **Use Encrypted Protocols:** Replace FTP with **SFTP** (SSH File Transfer Protocol) or **FTPS** (FTP over SSL/TLS) to encrypt credentials and data in transit.
2. **Disable Weak Services:** If file transfer is not required, port 21 should be closed.
3. **Patching:** Update the FTP daemon to a version later than 2.3.4 to remove the inherent backdoor risk.

---
*Disclaimer: This analysis was conducted in a private, isolated lab environment for educational purposes.*

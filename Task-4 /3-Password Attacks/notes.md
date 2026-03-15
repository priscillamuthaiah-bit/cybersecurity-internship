# Penetration Testing Milestone: Post-Exploitation & Password Cracking
**Target IP:** `192.168.56.103`  
**Phase:** Phase 3 (Post-Exploitation)  
**Tools:** John the Ripper (JTR), Unshadow, RockYou Wordlist

---

## 1. Password Hash Exfiltration
After gaining root access via the VSFTPD backdoor, I extracted the system's sensitive authentication files to perform offline password cracking.

### Files Captured:
- **`/etc/passwd`**: Contains user account information (stored as `metas2passwd.txt`).
- **`/etc/shadow`**: Contains encrypted password hashes (stored as `metas2shadow.txt`).

---

## 2. Methodology: Preparing the Hashes
Linux systems store user data and password hashes separately for security. To crack them, they must be combined into a format that cracking tools can interpret.

### The Unshadow Process:
I used the `unshadow` utility to merge the two files into a single hash file:
```bash
unshadow metas2passwd.txt metas2shadow.txt > myhashes.txt

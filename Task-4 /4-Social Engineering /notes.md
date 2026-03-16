# Internship Lab: Credential Harvesting & Phishing Simulation

## 1. Project Overview
This project demonstrates a simulated phishing attack using the **Social-Engineer Toolkit (SET)** within a controlled lab environment. The goal was to clone a login page and capture credentials to understand how these attacks function and how to mitigate them through user education.

---

## 2. Lab Environment
* **Attacker OS:** Kali Linux 2025.4
* **Attacker IP:** `192.168.56.102`
* **Target Application:** Damn Vulnerable Web Application (DVWA)
* **Target IP:** `192.168.56.103`

---

## 3. Attack Execution (Step-by-Step)

### Phase 1: Social-Engineer Toolkit (SET) Configuration
I utilized the **Credential Harvester Attack Method** via the Site Cloner tool.
1. **Selected Vector:** Website Attack Vectors -> Credential Harvester -> Site Cloner.
2. **POST Back IP:** Configured to `192.168.56.102` to receive the stolen data.
3. **Target URL:** Cloned `http://192.168.56.103/dvwa/login.php`.

### Phase 2: Intercepting Credentials
Once the "victim" entered credentials into the cloned site, the harvester successfully captured the plaintext data.

**Terminal Hit Logs:**
```text
[!] WE GOT A HIT! Printing the output:
POSSIBLE USERNAME FIELD FOUND: username=test
POSSIBLE PASSWORD FIELD FOUND: password=test
POSSIBLE LOGIN FIELD FOUND: Login=Login

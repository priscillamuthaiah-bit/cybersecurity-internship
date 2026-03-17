# Internship Project: Credential Harvesting & Phishing Awareness

## Project Overview
This project documents a simulated phishing and credential harvesting attack conducted in a controlled lab environment. The objective was to understand the mechanics of site cloning and form-data interception, followed by the creation of security awareness content for end-users.

---

## Lab Environment
* **Operating System:** Kali Linux 2025.4 (VirtualBox)
* **Target Application:** Damn Vulnerable Web Application (DVWA)
* **Networking:** * **Attacker IP:** `192.168.56.102`
    * **Target IP:** `192.168.56.103`
* **Tools Used:** Social-Engineer Toolkit (SET), GNU nano, Google Chrome.

---

## Phase 1: The Credential Harvesting Attack

### 1. Tool Configuration
I utilized the **Social-Engineer Toolkit (SET)** to launch a Website Attack Vector.
* **Attack Method:** Credential Harvester Attack Method.
* **Technique:** Site Cloner.
* **POST Back IP:** `192.168.56.102` (Local Kali instance).
* **Target URL to Clone:** `http://192.168.56.103/dvwa/login.php`.

### 2. Execution & Data Capture
SET successfully cloned the DVWA login page and hosted it on the attacker's IP. When a user enters credentials into the cloned site, the toolkit intercepts the HTTP POST request.

**Intercepted Data Output:**
```text
[!] WE GOT A HIT! Printing the output:
POSSIBLE USERNAME FIELD FOUND: username=test
POSSIBLE PASSWORD FIELD FOUND: password=test
POSSIBLE USERNAME FIELD FOUND: Login=Login

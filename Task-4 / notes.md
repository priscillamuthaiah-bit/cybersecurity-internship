# Internship Task 4: Exploitation & System Security

## 1. Objective
The goal of this task is to understand the complete penetration testing workflow, responsibly exploit vulnerabilities in a controlled environment, and implement defensive system hardening measures.

---

## 2. Penetration Testing Methodology
I followed a standard industry methodology to ensure a structured and professional assessment:
1.  **Reconnaissance:** Gathering initial information on the target.
2.  **Scanning:** Identifying open ports and active services.
3.  **Exploitation:** Actively gaining access via identified vulnerabilities.
4.  **Post-Exploitation:** Maintaining access and gathering deeper system data.
5.  **Reporting:** Documenting findings and suggesting mitigations.

---

## 3. Exploitation Lab (Metasploitable2)

### A. Metasploit Framework
I utilized the Metasploit Framework to exploit a known vulnerability in the Metasploitable2 target machine.
* **Reverse Shell:** Successfully gained system access by establishing a reverse shell.
* **Post-Exploitation Commands:**
    * `sysinfo`: Verified target system details.
    * `hashdump`: Extracted user password hashes for offline cracking.

### B. Password Attacks
* **Brute-Force:** Used **Hydra** to simulate a brute-force attack against the SSH service.
* **Hash Cracking:** Utilized **John the Ripper** to crack hashes obtained during the exploitation phase.

---

## 4. Social Engineering Simulation
I developed a phishing awareness simulation to demonstrate common attack vectors and defense strategies.
* **Phishing Page:** Created a simulated credential harvesting page using the Social-Engineer Toolkit (SET).
* **Awareness Training:** Provided educational content to help users detect red flags such as suspicious URLs and lack of HTTPS.

---

## 5. Malware Analysis & System Hardening

### A. Malware Fundamentals
Studied the behavior of malware through two primary methods:
* **Static Analysis:** Inspecting file properties and signatures without execution.
* **Dynamic Analysis:** Observing a benign sample in a secure **sandbox environment** to monitor behavioral changes.

### B. System Hardening Measures
Applied defensive configurations to secure the host machine:
* **Patch Management:** Ensured all security patches were applied to the operating system.
* **Firewall Configuration:** Configured `ufw` to block unauthorized malicious traffic.
* **Service Optimization:** Disabled all unused and unnecessary services to minimize the attack surface.

---

## 6. Key Takeaways & Mitigations
* **Least Privilege:** Unused services (like Apache2 when not in use) should be disabled to prevent exploitation.
* **Defense in Depth:** Combining proactive exploitation testing with reactive system hardening creates a resilient environment.
* **Responsible Disclosure:** All activities were performed within an isolated lab for educational purposes.

---
*Note: This documentation is part of the ApexPlanet Internship deliverables for Task 4.*

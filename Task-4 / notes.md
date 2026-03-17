# Internship Task 4: Exploitation & System Security

## 1. Objective
[cite_start]The goal of this task is to understand the complete penetration testing workflow, responsibly exploit vulnerabilities in a controlled environment, and implement defensive system hardening measures[cite: 195, 196].

---

## 2. Penetration Testing Methodology
[cite_start]I followed a standard industry methodology to ensure a structured and professional assessment[cite: 198]:
1.  [cite_start]**Reconnaissance:** Gathering initial information on the target[cite: 199].
2.  [cite_start]**Scanning:** Identifying open ports and active services[cite: 199].
3.  [cite_start]**Exploitation:** Actively gaining access via identified vulnerabilities[cite: 199].
4.  [cite_start]**Post-Exploitation:** Maintaining access and gathering deeper system data[cite: 199].
5.  [cite_start]**Reporting:** Documenting findings and suggesting mitigations[cite: 199, 200].

---

## 3. Exploitation Lab (Metasploitable2)

### A. Metasploit Framework
[cite_start]I utilized the Metasploit Framework to exploit a known vulnerability in the Metasploitable2 target machine[cite: 201, 202].
* [cite_start]**Reverse Shell:** Successfully gained system access by establishing a reverse shell[cite: 203].
* **Post-Exploitation Commands:**
    * [cite_start]`sysinfo`: Verified target system details[cite: 204].
    * [cite_start]`hashdump`: Extracted user password hashes for offline cracking[cite: 204].

### B. Password Attacks
* [cite_start]**Brute-Force:** Used **Hydra** to simulate a brute-force attack against the SSH service[cite: 206].
* [cite_start]**Hash Cracking:** Utilized **John the Ripper** to crack hashes obtained during the exploitation phase[cite: 207].

---

## 4. Social Engineering Simulation
[cite_start]I developed a phishing awareness simulation to demonstrate common attack vectors and defense strategies[cite: 208].
* [cite_start]**Phishing Page:** Created a simulated credential harvesting page (SET)[cite: 209].
* [cite_start]**Awareness Training:** Provided educational content to help users detect red flags such as suspicious URLs and lack of HTTPS[cite: 210].

---

## 5. Malware Analysis & System Hardening

### A. Malware Fundamentals
[cite_start]Studied the behavior of malware through two primary methods[cite: 216, 217]:
* [cite_start]**Static Analysis:** Inspecting file properties and signatures without execution[cite: 217].
* [cite_start]**Dynamic Analysis:** Observing a benign sample in a secure **sandbox environment** to monitor behavioral changes[cite: 217, 218].

### B. System Hardening Measures
[cite_start]Applied defensive configurations to secure the host machine[cite: 219]:
* [cite_start]**Patch Management:** Ensured all security patches were applied to the operating system[cite: 220].
* [cite_start]**Firewall Configuration:** Configured `ufw` to block unauthorized malicious traffic[cite: 221].
* [cite_start]**Service Optimization:** Disabled all unused and unnecessary services to minimize the attack surface[cite: 222].

---

## 6. Key Takeaways & Mitigations
* [cite_start]**Least Privilege:** Unused services (like Apache2 when not in use) should be disabled to prevent exploitation[cite: 222].
* [cite_start]**Defense in Depth:** Combining proactive exploitation testing with reactive system hardening creates a resilient environment[cite: 219, 221].
* [cite_start]**Responsible Disclosure:** All activities were performed within an isolated lab for educational purposes[cite: 196].

---

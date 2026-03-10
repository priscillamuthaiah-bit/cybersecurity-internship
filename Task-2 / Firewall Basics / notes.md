## 4. Defensive Hardening: Iptables Firewall Configuration

After identifying the massive attack surface through Nessus and Nmap, I implemented host-based firewall rules on the Metasploitable2 target to restrict unauthorized access.

### 4.1 Firewall Architecture
I manipulated the **Filter Table** within the **INPUT Chain** to manage incoming traffic. By default, the system was "Open," which I tightened using specific target actions (`ACCEPT`, `REJECT`, `DROP`).

### 4.2 Implemented Rules & Logic

| Goal | Command | Result |
| :--- | :--- | :--- |
| **Block SSH** | `sudo iptables -A INPUT -p tcp --dport 22 -j REJECT` | Prevents the Kali machine from even attempting an SSH connection. |
| **Web Access** | `sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT` | Keeps the Apache web server accessible for legitimate traffic. |
| **Attacker Ban** | `sudo iptables -A INPUT -s 192.168.56.102 -j DROP` | Silently discards all packets from the Attacker IP. |

### 4.3 Verification & Packet Counting
I verified the rules using the following command:
`sudo iptables -L -n -v`

**Observations:**
* **Chain INPUT:** I observed the packet and byte counters increasing, proving the firewall was actively intercepting traffic from the Kali machine.
* **Target REJECT:** Unlike `DROP`, which ignores the packet, `REJECT` sends a "Destination Port Unreachable" message back to the scanner, which I verified via a follow-up Nmap scan.

---

## 5. Summary of Findings
The lab successfully demonstrated the full security lifecycle:
1. **Discovery:** Identified 20+ open ports via Nmap.
2. **Analysis:** Confirmed 97 vulnerabilities and cleartext password leaks via Nessus/Wireshark.
3. **Mitigation:** Applied `iptables` rules to neutralize the primary attack vectors.

**Future Hardening:** Implement a "Default Drop" policy (`iptables -P INPUT DROP`) to ensure only explicitly allowed traffic enters the system.

# System Hardening – Security Implementation in Kali Linux

## Objective

The objective of this task was to strengthen the security of the operating system by implementing **system hardening techniques**. System hardening is the process of reducing vulnerabilities in a system by applying security updates, configuring firewall rules, and disabling unnecessary services that may expose the system to cyber threats.

This process helps protect systems from unauthorized access, malware attacks, and network-based exploits.

---

# What is System Hardening?

System hardening refers to securing a system by **reducing its attack surface**. This is done by:

- Installing security patches
- Configuring firewall protection
- Disabling unnecessary services
- Monitoring open ports and network connections

The goal is to make the system **less vulnerable to cyber attacks**.

---

# Environment Used

Operating System: Kali Linux

Tools Used:

- `apt` (package management)
- `ufw` (Uncomplicated Firewall)
- `systemctl` (service management)
- `netstat` (network monitoring)

---


# Step 1 – Update System Packages

## Command
```
sudo apt update
```

## Explanation
This command updates the package list from the Kali Linux repositories. It ensures the system knows about the latest available software and security updates.

---

# Step 2 – Apply Security Updates

## Command
```
sudo apt upgrade -y
```

## Explanation
This installs the latest versions of all installed packages. Security patches fix vulnerabilities that attackers could exploit.

---

# Step 3 – Install Firewall (UFW)

## Command
```
sudo apt install ufw
```

## Explanation
UFW (Uncomplicated Firewall) is used to manage firewall rules in Linux. Installing it allows us to control network traffic entering and leaving the system.

---

# Step 4 – Enable Firewall

## Command
```
sudo ufw enable
```

## Explanation
This command activates the firewall and ensures it runs automatically at system startup.

---

# Step 5 – Block Incoming Connections

## Command
```
sudo ufw default deny incoming
```

## Explanation
This blocks all incoming connections by default. External systems will not be able to access the machine unless explicitly allowed.

---

# Step 6 – Allow Outgoing Connections

## Command
```
sudo ufw default allow outgoing
```

## Explanation
This allows the system to initiate outgoing connections such as accessing websites or downloading updates.

---

# Step 7 – Allow SSH Access

## Command
```
sudo ufw allow ssh
```

## Explanation
SSH allows secure remote login to the system. This rule allows SSH connections while keeping other incoming connections blocked.

---

# Step 8 – Check Firewall Status

## Command
```
sudo ufw status verbose
```

## Explanation
This command displays firewall status, default rules, and allowed services. It confirms that the firewall is properly configured.

---

# Step 9 – List Installed Services

## Command
```
systemctl list-unit-files --type=service
```

## Explanation
This command lists all services installed on the system. Some services may not be necessary and could increase security risks.

---

# Step 10 – Check Running Services

## Command
```
systemctl list-units --type=service --state=running
```

## Explanation
This displays services currently running on the system. Running services may open network ports or create vulnerabilities.

---

# Step 11 – Disable Unnecessary Services

## Example Commands
```
sudo systemctl disable apache2
sudo systemctl stop apache2
```

## Explanation
Disabling unused services prevents them from starting automatically. Stopping them immediately removes potential entry points for attackers.

---

# Step 12 – Monitor Open Ports

## Command
```
netstat -tulnp
```

## Explanation
This command shows open ports, active network connections, and the processes using them. Monitoring ports helps detect suspicious activity.

---

# Result

After completing this task:

- System packages were updated and security patches applied
- Firewall was installed and configured
- Incoming traffic was restricted
- Necessary services were allowed
- Unused services were disabled
- Network ports were monitored

These actions improved the overall security posture of the system.

---

# Conclusion

System hardening is an essential security practice in cybersecurity. By applying updates, configuring firewall rules, managing system services, and monitoring network activity, the system becomes more resistant to attacks and unauthorized access.

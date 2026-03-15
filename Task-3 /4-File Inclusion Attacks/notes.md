# Internship Lab Notes: Web Vulnerability Research & Remediation

## 1. Project Overview
This repository documents a series of penetration testing labs conducted in a controlled environment (**DVWA on Kali Linux**). The focus is on identifying common web vulnerabilities (XSS, CSRF, File Inclusion) and implementing manual, code-level remediation.

---

## 2. File Inclusion (LFI/RFI)
**Definition:** File Inclusion vulnerabilities allow an attacker to include files on a web server through the web browser. This occurs when an application uses an input path to a file as a parameter without proper sanitization.

### 2.1 Local File Inclusion (LFI) - Low Level
* **Vulnerability:** The `page` parameter in the URL is directly passed to the PHP `include()` function.
* **Exploitation:** Used directory traversal sequences (`../`) to access sensitive system files.
* **Payload:** `?page=../../../../../../etc/passwd`
* **Result:** Successfully exfiltrated the `/etc/passwd` file, revealing system user information.
* **Lab Objective:** Accessed hidden flags in `../../hackable/flags/fi.php`.

### 2.2 Manual Remediation (The Fix)
The "Low" level code simply takes the GET input: `$file = $_GET['page'];`. To harden this, I implemented a **White-list** approach.

**Secured Implementation:**
```php
<?php
$file = $_GET[ 'page' ];

// THE FIX: Only allow specific, known files to be included
if( $file != "include.php" && $file != "file1.php" && $file != "file2.php" && $file != "file3.php" ) {
    // If the input doesn't match the white-list, force a safe default or error
    echo "ERROR: File not found.";
    exit;
}
?>

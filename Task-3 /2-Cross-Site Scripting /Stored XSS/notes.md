# Web Vulnerability Research: XSS Exploitation & Manual Patching

## 1. Project Overview
This document details the identification, exploitation, and manual remediation of Cross-Site Scripting (XSS) vulnerabilities within the **DVWA (Damn Vulnerable Web Application)** environment. The goal was to transition the application from a "Low" security state to a hardened state through manual source code modification.

---

## 2. Reflected XSS (Non-Persistent)

### 2.1 Vulnerability Discovery
In the Reflected XSS module, the application takes a string from the `name` parameter in the URL and reflects it directly into the page's HTML structure. 

* **Attack Vector:** HTTP GET Request
* **Payload Used:** `<script>alert('Reflected_XSS_Success')</script>`
* **Result:** The browser interpreted the injected string as executable JavaScript, triggering an alert box.

### 2.2 Terminal-Based Remediation
Using the terminal, I accessed the server's backend to apply a permanent fix.

* **File Path:** `/var/www/html/vulnerabilities/xss_r/source/low.php`
* **Tools:** `nano` editor

**Applied Patch:**
I introduced `htmlspecialchars()` to the output logic. This ensures that characters like `<` and `>` are converted to HTML entities (`&lt;` and `&gt;`), rendering them harmless.

```php
// Original Vulnerable Code:
// echo "Hello " . $_GET[ 'name' ];

// Fixed Hardened Code:
$name = htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8');
echo "<pre>Hello " . $name . "</pre>";

Applied Patch:
// Step 1: Trim whitespace to prevent bypasses
$message = trim($_POST['mtxMessage']);
$name    = trim($_POST['txtName']);

// Step 2: Sanitize and Encode
// ENT_QUOTES handles both single and double quotes
$message = htmlspecialchars($message, ENT_QUOTES, 'UTF-8');
$name    = htmlspecialchars($name, ENT_QUOTES, 'UTF-8');

// Step 3: Insert into Database
$query  = "INSERT INTO guestbook ( comment, name ) VALUES ( '$message', '$name' );";

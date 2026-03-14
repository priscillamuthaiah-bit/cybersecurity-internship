# DVWA Lab: Stored Cross-Site Scripting (XSS) - Low Level

## 1. Vulnerability Overview
**Stored XSS** (Persistent) occurs when an application receives data from a user, stores it in a database without validation, and later reflects that data back to other users. In this lab, the **Guestbook** was used to host a persistent malicious script.

---

## 2. Technical Lab Steps (Exploitation)

### Environment
* **Application:** DVWA (Damn Vulnerable Web Application)
* **Security Level:** Low
* **Vulnerable Module:** XSS (Stored)
* **Vulnerable Parameters:** `txtName` (Name) and `mtxMessage` (Message)

### Exploitation Process
1. **Injection:** Navigated to the Guestbook and entered a standard JavaScript alert payload into the message field.
   ```javascript
   <script>alert('Vulnerable to Stored XSS');</script>

File: vulnerabilities/xss_s/source/low.php

PHP
<?php
if( isset( $_POST[ 'btnSign' ] ) ) {
    // Input is trimmed but NOT sanitized or encoded
    $message = trim( $_POST[ 'mtxMessage' ] );
    $name    = trim( $_POST[ 'txtName' ] );

    // Vulnerable SQL query
    $query  = "INSERT INTO guestbook ( comment, name ) VALUES ( '$message', '$name' );";
    // ... execution code ...
}
?>

The Fix

<?php
if( isset( $_POST[ 'btnSign' ] ) ) {
    // FIX: Apply htmlspecialchars() to sanitize input
    $message = htmlspecialchars(trim($_POST['mtxMessage']), ENT_QUOTES, 'UTF-8');
    $name    = htmlspecialchars(trim($_POST['txtName']), ENT_QUOTES, 'UTF-8');

    // Secure insertion
    $query  = "INSERT INTO guestbook ( comment, name ) VALUES ( '$message', '$name' );";
    // ... execution code ...
}
?>

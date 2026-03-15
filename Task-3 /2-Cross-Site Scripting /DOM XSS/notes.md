# Technical Lab Notes: DOM-Based XSS Analysis & Remediation

## 1. Vulnerability Overview
**DOM-Based XSS** is a unique client-side vulnerability. Unlike Stored or Reflected XSS, the entire attack happens in the browser's Document Object Model (DOM). The server side often never sees the malicious payload; instead, the client-side JavaScript reads data from a "source" (like the URL) and passes it to a "sink" (an execution point like `innerHTML`).



---

## 2. Exploitation (Low Level)

### Identification
The vulnerability was identified in the language selection feature. The application reads the `default` parameter from the URL and writes it to the page to set the current language.

### Attack Execution
* **Source:** `window.location.search`
* **Vulnerable Parameter:** `default`
* **Payload:** `default=English<script>alert('DOM_XSS_Success')</script>`

By appending the script tag to the URL parameter, the browser's JavaScript engine parsed the string and executed the alert, confirming that the input was treated as code rather than a simple string.

---

## 3. Manual Remediation (The Patch)

To secure the "Low" level manually, I accessed the server via the terminal and implemented **White-Listing** to ensure only authorized inputs are processed.

### File Path: 
`/var/www/html/DVWA/vulnerabilities/xss_d/source/low.php`

### The Secure Implementation:
I modified the PHP logic to intercept the `GET` request and validate the language choice before the page loads.

```php
<?php
// THE FIX: Server-side White-Listing
if (isset($_GET['default'])) {
    $lang = $_GET['default'];
    
    // Define allowed values
    switch ($lang) {
        case "French":
        case "English":
        case "German":
        case "Spanish":
            // Safe language selected, allow execution
            break;
        default:
            // If any unknown string (or script) is detected, force a safe redirect
            header("location: ?default=English");
            exit;
    }
}
?>

# Vulnerability Lab Notes: Cross-Site Scripting (XSS)

## 1. Stored XSS (Persistent)
Stored XSS occurs when a payload is saved in the database and executed every time a user visits the page.

### Lab Evidence (Low Level)
* **Payload:** `<script>alert('Success');</script>`
* **Execution:** Successfully triggered a browser alert box upon page refresh, confirming the script was stored in the guestbook database.
* **Vulnerability:** The backend fails to sanitize the `mtxMessage` parameter.

### Lab Evidence (Impossible Level)
* **Observation:** The script is rendered as plain text: `&lt;script&gt;alert('Success');&lt;/script&gt;`
* **Mechanism:** The application uses `htmlspecialchars()` to encode output, neutralizing the script tags.

---

## 2. Reflected XSS (Non-Persistent)
Reflected XSS occurs when a script is "reflected" off the web server via a URL parameter or form submission.

### Lab Evidence (Low Level)
* **Target URL:** `http://127.0.0.1:42001/vulnerabilities/xss_r/?name=<script>alert('hi')</script>`
* **Execution:** Entering the script into the "What's your name?" field reflected the alert box back to the browser immediately.
* **Vulnerability:** The PHP code uses `echo` directly on the `$_GET['name']` variable without encoding.

---

## 3. Code Remediation (The Fix)

I manually patched the `low.php` source files to demonstrate how to secure these vulnerabilities.

### File: `vulnerabilities/xss_r/source/low.php`

**Vulnerable Code:**
```php
echo '<pre>Hello ' . $_GET[ 'name' ] . '</pre>';

Secured Code (My Implementation):
// THE FIX: Use htmlspecialchars to encode the input
// This converts <script> into &lt;script&gt; so the browser treats it as text.
$name = htmlspecialchars( $_GET[ 'name' ], ENT_QUOTES, 'UTF-8' );

echo '<pre>Hello ' . $name . '</pre>';

# Technical Lab Notes: Cross-Site Request Forgery (CSRF)

## 1. Vulnerability Overview
**Cross-Site Request Forgery (CSRF)** is an attack that forces an authenticated user to execute unwanted actions on a web application in which they are currently authenticated. With a little help of social engineering (such as sending a link via email or chat), an attacker may trick the users of a web application into executing actions of the attacker's choosing.

[Image of Cross-Site Request Forgery (CSRF) attack workflow]

---

## 2. Exploitation (Security: Low)

### Identification
At the **Low** security level, the application does not verify if the request was intentionally sent by the user. The password change form uses a `GET` request, meaning all parameters are visible and easily manipulated in the URL.

### Attack Execution
I successfully simulated a CSRF attack by crafting a malicious URL that changes the user's password without requiring their current password or any confirmation.

* **Payload URL:** `http://127.0.0.1:42001/vulnerabilities/csrf/?password_new=hacker123&password_conf=hacker123&Change=Change`
* **Method:** By visiting this URL while logged into the DVWA session, the password was instantly updated to "hacker123".
* **Evidence:** The application returned the message: *"Password Changed."*

---

## 3. Defense Analysis (Security: Impossible)

The **Impossible** level implements the industry-standard defense against CSRF: **Anti-CSRF Tokens**.

### How it Works:
1. When the user loads the password change page, the server generates a unique, cryptographically strong, random string (the **Token**).
2. This token is stored in the user's session and also placed in a hidden field in the HTML form.
3. When the user submits the form, the server compares the token in the request with the token in the session.
4. If they match, the request is accepted. If they don't (or if the token is missing), the request is rejected.

### Code Evidence (Impossible Level):
```php
// Verification of the Anti-CSRF token
if( $_REQUEST[ 'user_token' ] === $_SESSION[ 'session_token' ] ) {
    // Proceed

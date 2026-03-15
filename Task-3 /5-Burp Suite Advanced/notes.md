# Internship Project: Web Application Penetration Testing
**Date:** 2026-03-15  
**Tooling:** Burp Suite Community Edition, Kali Linux, DVWA  

---

## 1. Intercepting and Modifying Login Requests

### Objective
Demonstrate the ability to intercept client-side traffic and manipulate request parameters before they reach the server.

### Steps Taken
1. **Proxy Configuration:** Launched Burp's internal browser with the sandbox disabled to bypass OS restrictions.
2. **Interception:** Enabled `Intercept is on` in the **Proxy** tab.
3. **Capture:** Submitted a login attempt with the following placeholder data:
   - **Username:** `test`
   - **Password:** `test`
4. **Manipulation:** Modified the raw HTTP POST body in Burp Suite:
   - Changed `username=test` to `username=admin`
   - Changed `password=test` to `password=password`
5. **Forwarding:** Released the request. The server processed the modified credentials, successfully bypassing the original user input.

---

## 2. Automated Fuzzing with Burp Intruder

### Objective
Perform an automated brute-force/fuzzing attack against the login endpoint to identify valid credentials.

### Methodology
- **Attack Type:** Sniper
- **Target Parameter:** `password`
- **Payload Type:** Simple List (Commonly used passwords)

### Execution
1. Sent the intercepted login request to **Intruder**.
2. Cleared default positions and set the payload marker on the password value: `password=§test§`.
3. Loaded a wordlist containing over 1,000 common passwords.
4. Analyzed the **Results** window for outliers:
   - **Status Code:** Looked for `302 Redirect` (indicating a successful login redirect).
   - **Length:** Monitored for variations in response size, which indicates a different server response (Success vs. Failure).

---

##

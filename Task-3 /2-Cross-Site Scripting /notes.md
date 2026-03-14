# XSS Vulnerability Documentation

## 1. Stored XSS (Persistent)
* **Description:** Malicious script is permanently stored on the server.
* **Impact:** Affects every user who views the page where the script is stored.
* **DVWA Example:** Found in the "XSS (Stored)" module, typically using the Guestbook.
* **Payload:** `<script>alert('Stored XSS')</script>`

## 2. Reflected XSS (Non-Persistent)
* **Description:** Script is delivered via a URL parameter and reflected back in the response.
* **Impact:** Requires the attacker to trick a specific victim into clicking a link.
* **DVWA Example:** Found in the "XSS (Reflected)" module where a name parameter is echoed.
* **Payload:** `?name=<script>alert(document.cookie)</script>`

## 3. DOM-based XSS
* **Description:** The vulnerability is in the client-side JavaScript, not the server-side code.
* **Impact:** Execution happens entirely in the browser through the Document Object Model.
* **DVWA Example:** Found in the "XSS (DOM)" module using the language selection parameter.
* **Payload:** `?default=English<script>alert('DOM')</script>`

## Remediation Strategies
1.  **Output Encoding:** Convert special characters into HTML entities (e.g., `<` becomes `&lt;`) before rendering in the browser.
2.  **Input Validation:** Use "Allow-lists" to ensure only expected data formats are processed.
3.  **Content Security Policy (CSP):** Implement a strong CSP header to restrict where scripts can be loaded and executed from.
4.  **Use Modern Frameworks:** Use frameworks like React or Angular that perform automatic contextual encoding.

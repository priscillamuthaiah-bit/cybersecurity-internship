# Technical Notes: SQL Injection (SQLi)

## 1. Vulnerability Analysis
The vulnerability exists because the application uses **string concatenation** to build SQL queries. This allows user input to "break out" of the data context and enter the command context.

### The Attack Vector
- **Entry Point:** `id` parameter via GET request.
- **SQL Driver:** MySQL/MariaDB.
- **Vulnerability Type:** Error-based / Union-based SQLi.

## 2. Payload Breakdown

### Basic Tautology
`' OR '1'='1`
- **Purpose:** Forces the `WHERE` clause to always evaluate to TRUE.
- **Result:** Returns all rows from the current table.

### UNION-Based Extraction
`%' UNION SELECT user, password FROM users #`
- `%`: Acts as a wildcard for the original query.
- `UNION SELECT`: Combines the results of the original query with a new one.
- `user, password`: The specific columns we want to leak.
- `FROM users`: The target table containing credentials.
- `#`: The MySQL comment character; it nullifies the rest of the original developer's query to prevent syntax errors.

## 3. Cryptography Observations
The application stores passwords using the **MD5 (Message Digest 5)** algorithm.
- **Type:** 128-bit hash.
- **Weakness:** High collision rate and susceptible to fast "brute-forcing" via Rainbow Tables.
- **Status:** Deprecated. Should be replaced with `bcrypt` or `Argon2`.

## 4. Remediation Deep Dive

### Prepared Statements (The Fix)
Prepared statements ensure that the database treats user input as a **literal value** rather than executable code.

**How it works:**
1. The SQL template is sent to the DB (`SELECT... WHERE id = ?`).
2. The DB compiles the logic.
3. The user data is sent later and bound to the `?`.

Even if the data contains SQL commands, the DB engine has already finished "parsing" the command, so the injection is ignored.

## 5. Useful Commands for this Lab
- **Find DVWA files in Kali:** `locate low.php`
- **Check MD5 hash type:** `hashid <hash_value>`
- **John the Ripper (Local Cracking):** `john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt`

---
*Internship Note: Documenting these steps ensures

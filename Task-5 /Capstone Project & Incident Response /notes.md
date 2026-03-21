# Security Awareness Phishing Simulation
**Apex Planet Cybersecurity Internship**

> **Tools:** Gophish v0.12.1 · MailHog (Docker) · Kali Linux  
> **Date:** March 19–21, 2026  
> **Environment:** Isolated lab – 127.0.0.1 (loopback only)  
> **MITRE ATT&CK:** T1566 (Phishing), T1598.003 (Spearphishing Link)

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Objectives and Scope](#2-objectives-and-scope)
3. [Tools and Architecture](#3-tools-and-architecture)
4. [Setup and Installation](#4-setup-and-installation)
5. [Campaign Configuration](#5-campaign-configuration)
6. [Campaign Execution](#6-campaign-execution)
7. [Results and Findings](#7-results-and-findings)
8. [Mitigation Strategies](#8-mitigation-strategies)
9. [Incident Response Simulation](#9-incident-response-simulation)
10. [Screenshots Reference](#10-screenshots-reference)
11. [References](#11-references)

---

## 1. Project Overview

This project is a controlled phishing simulation done as part of the Apex Planet Cybersecurity Internship. The idea was to go through the full lifecycle of a phishing attack — from setting up the tools, to crafting the email, to seeing what happens when a target clicks the link and submits credentials — all in a safe, isolated environment.

I used **Gophish** as the phishing framework and **MailHog** as a fake SMTP server to catch all outgoing emails locally. The whole thing ran on loopback (127.0.0.1) so nothing ever reached the real internet or any real user.

The simulation covered:
- Infrastructure setup (Gophish + MailHog)
- Reconnaissance (defining a target)
- Creating a phishing email and fake login page
- Launching the campaign and tracking results
- Incident response walkthrough
- Mitigation recommendations

> **Legal note:** This was done entirely in a controlled lab environment. No real users, production systems, or internet-facing infrastructure were involved. Always get written permission before running phishing simulations outside a lab.

---

## 2. Objectives and Scope

### What I was trying to achieve

- Set up a working phishing simulation framework from scratch
- Successfully deliver a phishing email and capture credential submission
- Understand and document each phase of the attack chain
- Demonstrate how an incident response team would respond
- Write up practical mitigation recommendations

### Scope

| Parameter | Value |
|-----------|-------|
| Environment | Isolated – localhost only (127.0.0.1) |
| Target | Synthetic: Test User `<test@test.com>` |
| Gophish admin | https://127.0.0.1:3333 |
| Gophish listener | http://127.0.0.1:80 |
| MailHog SMTP | 127.0.0.1:1025 |
| MailHog web UI | http://127.0.0.1:8025 |
| OS | Kali Linux |
| Out of scope | All real users and production systems |

---

## 3. Tools and Architecture

### Tool Stack

| Tool | Version | Purpose |
|------|---------|---------|
| Gophish | v0.12.1 | Phishing framework – emails, landing pages, tracking |
| MailHog | Latest (Docker) | Fake SMTP server that catches all sent emails locally |
| Docker | System | Running MailHog in a container |
| Kali Linux | Rolling | Main OS |
| Firefox | Latest | Accessing Gophish and MailHog UIs |

### Lab Network Diagram

```
 ┌─────────────────────────────────────────────────────────┐
 │            Isolated Lab  –  127.0.0.1 (loopback)        │
 │                                                          │
 │  ┌───────────────┐  SMTP :1025   ┌──────────────────┐  │
 │  │  Kali Linux   │──────────────▶│ MailHog (Docker) │  │
 │  │  (attacker)   │               │  Web UI  :8025   │  │
 │  └───────┬───────┘               └──────────────────┘  │
 │          │                                               │
 │          │ launch campaign       ┌──────────────────┐  │
 │          └──────────────────────▶│ Gophish v0.12.1  │  │
 │                                  │  Admin   :3333   │  │
 │                                  │  Listen  :80     │  │
 │                                  └────────┬─────────┘  │
 │                                           │              │
 │                                           ▼              │
 │                                  ┌──────────────────┐  │
 │                                  │   Test User      │  │
 │                                  │  test@test.com   │  │
 │                                  └──────────────────┘  │
 └─────────────────────────────────────────────────────────┘
```

---

## 4. Setup and Installation

### Step 1 – Start MailHog

```bash
sudo docker run -d -p 1025:1025 -p 8025:8025 mailhog/mailhog
```

- Port **1025** → Gophish sends emails here
- Port **8025** → Open in browser to see the inbox: `http://127.0.0.1:8025`

Verify it is running:
```bash
sudo docker ps | grep mailhog
```

---

### Step 2 – Download and Run Gophish

```bash
cd ~/Downloads

# Download
wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip

# Extract
unzip gophish-v0.12.1-linux-64bit.zip

# Check contents
ls -la

# Make executable
chmod +x gophish

# Run (needs root for port 80)
sudo ./gophish
```

On first run you will see something like:
```
Please login with the username admin and the password: <auto-generated-password>
Starting phishing server at http://0.0.0.0:80
Starting admin server at https://127.0.0.1:3333
```

Copy the auto-generated password – it only appears once.

**Admin panel:** `https://127.0.0.1:3333`  
Username: `admin`  
Password: (from terminal output)

---

### Step 3 – Create a Sending Profile

Go to **Sending Profiles → New Profile**

```
Name:                      MailHog
Interface Type:            SMTP
SMTP From:                 Test User <test@demo.com>
Host:                      127.0.0.1:1025
Username:                  (leave blank)
Password:                  (leave blank)
Ignore Certificate Errors: checked
```

Click **Save Profile**.

---

## 5. Campaign Configuration

### 5.1 Create a Landing Page

Go to **Landing Pages → New Page**

1. Give it a name: `Login Page`
2. Click **Import Site**
3. Enter the URL of the site you want to clone (e.g. a login page)
4. Enable **Capture Submitted Data**
5. Enable **Capture Passwords**
6. Set **Redirect to:** the real site (so the user gets redirected after submitting)
7. Click **Save Page**

The imported page looks identical to the real login page. Any credentials submitted get logged in Gophish.

---

### 5.2 Create an Email Template

Go to **Email Templates → New Template**

| Field | Value |
|-------|-------|
| Name | Password Reset |
| Subject | `Urgent: Reset Your Password` |
| From | `"Test User" <test@test.com>` |

Body (HTML):
```html
Hello,

We detected unusual activity on your account

Please reset your password immediately:

<a href="{{.URL}}">Reset Password</a>

Thank you.
```

The `{{.URL}}` variable gets replaced by Gophish with a unique tracked URL for each recipient. Add `<img src="{{.TrackingURL}}" width="1" height="1">` somewhere in the body to track email opens.

**Gophish template variables:**
| Variable | What it inserts |
|----------|----------------|
| `{{.FirstName}}` | Target's first name |
| `{{.LastName}}` | Target's last name |
| `{{.Email}}` | Target's email address |
| `{{.URL}}` | Unique phishing link for this recipient |
| `{{.TrackingURL}}` | 1-pixel tracking image URL |

---

### 5.3 Create a User Group

Go to **Users & Groups → New Group**

```
Group Name: Test Group
```

Add a member or import via CSV:
```csv
First Name,Last Name,Email,Position
Test,User,test@test.com,Employee
```

In a real engagement you would import a CSV of targets collected during OSINT.

---

### 5.4 Send a Test Email First

Before launching, always validate delivery:

1. Go to the campaign creation form
2. Click **Send Test Email** next to the sending profile
3. Fill in test details
4. Check MailHog at `http://127.0.0.1:8025` to confirm it arrived

This saves time debugging SMTP issues after the campaign is already running.

---

## 6. Campaign Execution

Go to **Campaigns → New Campaign**

| Field | Value |
|-------|-------|
| Name | Test Campaign |
| Email Template | Password Reset |
| Landing Page | Login Page |
| URL | `http://127.0.0.1` |
| Launch Date | (now) |
| Sending Profile | MailHog |
| Groups | Test Group |

Click **Launch Campaign**. You will see a "Campaign Scheduled!" confirmation.

---

### Watching Results

The campaign dashboard updates in real time:

| Metric | What triggers it |
|--------|-----------------|
| Email Sent | Gophish handed the email to MailHog |
| Email Opened | The tracking image in the email was loaded |
| Clicked Link | Target visited the `{{.URL}}` phishing link |
| Submitted Data | Target submitted the form on the landing page |
| Email Reported | Target used the Gophish report button |

**Campaign 3 – actual timeline from this simulation:**
```
06:47:25 – Email Sent      → test@test.com
06:47:30 – Email Opened    → tracking pixel loaded
06:47:45 – Clicked Link    → http://127.0.0.1/?rid=g3ANPfB
06:47:55 – Submitted Data  → credentials captured on fake login page
```

All four events happened within about 90 seconds.

---

### Export Results

```
Campaigns → [Campaign Name] → Export CSV → Results
```

The CSV contains per-user events with timestamps, IP addresses, and user-agent strings.

---

## 7. Results and Findings

### Campaign Summary

| Metric | Campaign 1 (initial test) | Campaign 3 (full simulation) |
|--------|--------------------------|------------------------------|
| Email Sent | 1 | 1 |
| Email Opened | 0 | 1 (100%) |
| Link Clicked | 0 | 1 (100%) |
| Data Submitted | 0 | 1 (100%) |
| Email Reported | 0 | 0 |

### Key Findings

**Finding 1 – HIGH: Full attack chain completed, credentials captured**  
In Campaign 3 the target went through the complete flow: email opened → link clicked → credentials submitted. This confirms the simulation worked end to end.

**Finding 2 – HIGH: Infrastructure ready in under 10 minutes**  
Both tools were running in less than 10 minutes using free, open-source software. The barrier to running a phishing operation is very low.

**Finding 3 – HIGH: Login page cloned with one click**  
The Import Site feature clones any login page instantly and the result is visually identical to the real site.

**Finding 4 – HIGH: Spoofed email delivered without filtering**  
The From address was spoofed with no SPF, DKIM, or DMARC checks blocking it.

**Finding 5 – MEDIUM: No MFA on Gophish admin**  
Admin panel protected by username/password only.

**Finding 6 – MEDIUM: Landing page on HTTP**  
No TLS certificate. A real attacker would add HTTPS to appear more legitimate.

**Finding 7 – LOW: Target did not report the email**  
Email Reported stayed at 0, meaning no user-initiated alert to the security team.

---

## 8. Mitigation Strategies

### Technical

**SPF / DKIM / DMARC**

SPF (DNS TXT record):
```
v=spf1 include:_spf.google.com ip4:203.0.113.10 -all
```

DMARC (DNS TXT record):
```
v=DMARC1; p=reject; rua=mailto:dmarc@company.com; pct=100
```

- SPF: Lists authorised sending servers for your domain
- DKIM: Cryptographically signs emails so they cannot be forged
- DMARC: Enforces both and tells receiving servers what to do with failures (`reject` is strongest)

**Multi-factor authentication (MFA)**  
Enforce on all accounts. Even if credentials are stolen, MFA stops the attacker logging in. FIDO2 hardware keys are the most phishing-resistant option.

**Email security gateway**  
Microsoft Defender for Office 365, Proofpoint, or Mimecast can rewrite URLs, sandbox attachments, and detect impersonation before email reaches the inbox.

**DNS and web filtering**  
Block newly registered domains and phishing infrastructure at DNS/proxy level. Stops users reaching the landing page even after clicking.

### Procedural

**Phishing awareness training** – run at least twice a year, covering sender address inspection, link hovering, and credential request red flags.

**Regular simulated phishing campaigns** – quarterly Gophish runs with targeted follow-up training for users who click.

**Easy reporting process** – "Report Phishing" button in the email client, no blame culture for clicking accidentally.

**Least privilege** – limit account access so a compromised account causes minimum damage.

---

## 9. Incident Response Simulation

Framework: **NIST SP 800-61 Rev. 2**

```
Preparation → Detection → Containment → Eradication → Recovery → Post-Incident Review
```

### Detection Indicators

```
# Gophish campaign events
06:47:25 – email_sent      target: test@test.com
06:47:30 – email_opened    ip: 127.0.0.1
06:47:45 – clicked_link    url: http://127.0.0.1/?rid=g3ANPfB
06:47:55 – submitted_data  credentials captured

# What to look for in a real environment
- SPF/DKIM fail on inbound email at the mail gateway
- Proxy log: user visiting an unknown or newly registered domain
- POST to /login from an unusual IP after a phishing email was delivered
- SIEM alert: login attempt from new location shortly after phishing delivery
```

### Containment Steps

**Immediate (0–30 min)**
- Lock the affected user account
- Block the phishing URL at web proxy and DNS
- Alert all staff
- Quarantine similar emails from user inboxes

**Short-term (30 min – 4 hrs)**
- Force password reset for anyone who submitted credentials
- Revoke active sessions and OAuth tokens
- Delete phishing email from all mailboxes via admin tools
- Submit takedown request for the phishing domain

**Recovery (4 hrs – 48 hrs)**
- Restore account after password reset + MFA enrolment
- Monitor account for anomalous logins for 30 days
- Scan affected endpoints with EDR tool
- Add phishing domain/IP to threat intel feeds

### Post-Incident Report Template

```markdown
## Post-Incident Report

Incident ID:  INC-2026-001
Date:         2026-03-19
Severity:     High (credentials submitted)
Status:       Closed

### Summary
A phishing email impersonating an IT security notice was delivered to
test@test.com. The target opened the email, clicked the link, and
submitted credentials on a fake login page.

### Timeline
06:47:25  Email delivered
06:47:30  Email opened
06:47:45  Phishing link clicked
06:47:55  Credentials submitted on fake login page

### Impact
- 1 user account credential compromised
- No data exfiltration (lab environment)

### Root Cause
- No DMARC policy enforced
- No MFA on user accounts
- No email security gateway

### Actions Taken
1. User account locked
2. Phishing URL blocked
3. Password reset issued
4. MFA enrolment required

### Lessons Learned
- Implement DMARC p=reject
- Deploy MFA for all accounts
- Add email security gateway

### Follow-up Tasks
| Task | Owner | Deadline |
|------|-------|----------|
| Enable DMARC reject | IT Admin | 2026-03-26 |
| MFA for all accounts | IT Admin | 2026-03-31 |
| Email gateway | CISO | 2026-04-15 |
| Phishing simulation programme | SOC | 2026-04-01 |
```

---

## 10. Screenshots Reference

| Figure | Filename | Description |
|--------|----------|-------------|
| Fig 1 | Screenshot_2026-03-19_151659.png | wget – downloading Gophish |
| Fig 2 | Screenshot_2026-03-19_151728.png | unzip – extracting the archive |
| Fig 3 | Screenshot_2026-03-19_152023.png | chmod +x and running Gophish; DB migrations |
| Fig 4 | Screenshot_2026-03-19_152258.png | Gophish admin dashboard |
| Fig 5 | Screenshot_2026-03-19_152354.png | MailHog web UI (empty inbox) |
| Fig 6 | Screenshot_2026-03-19_152658.png | New Sending Profile form |
| Fig 7 | Screenshot_2026-03-19_152953.png | Sending Profile – lower section |
| Fig 8 | Screenshot_2026-03-19_153248.png | Sending Profile saved |
| Fig 9 | Screenshot_2026-03-19_153326.png | Sending Profiles list |
| Fig 10 | Screenshot_2026-03-19_153524.png | Import Site dialog |
| Fig 11 | Screenshot_2026-03-19_153541.png | Landing Pages list |
| Fig 12 | Screenshot_2026-03-19_154354.png | Email Templates list |
| Fig 13 | Screenshot_2026-03-19_154649.png | Users & Groups – Test Group |
| Fig 14 | Screenshot_2026-03-19_155616.png | Test email sent confirmation |
| Fig 15 | Screenshot_2026-03-19_155738.png | New Campaign form |
| Fig 16 | Screenshot_2026-03-19_155957.png | MailHog – first test email |
| Fig 17 | Screenshot_2026-03-19_160025.png | Campaign 1 results: 1 sent, 0 opened |
| Fig 18 | Screenshot_2026-03-19_160049.png | MailHog inbox with both emails |
| Fig 19 | Screenshot_2026-03-19_160107.png | Email body – phishing content and reset link |
| Fig 20 | Screenshot_2026-03-19_161509.png | Campaign Scheduled confirmation |
| Fig 21 | Screenshot_2026-03-19_161800.png | Phishing landing page at /?rid=g3ANPfB |
| Fig 22 | Screenshot_2026-03-19_162329.png | Campaign 3 results: all 4 events triggered |

---

## 11. References

| Resource | URL |
|----------|-----|
| Gophish documentation | https://docs.getgophish.com |
| Gophish GitHub | https://github.com/gophish/gophish |
| MailHog GitHub | https://github.com/mailhog/MailHog |
| MITRE ATT&CK – Phishing | https://attack.mitre.org/techniques/T1566/ |
| NIST SP 800-115 | https://csrc.nist.gov/publications/detail/sp/800-115/final |
| NIST SP 800-61 Rev 2 | https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final |
| DMARC overview | https://dmarc.org/overview/ |
| Google MX toolbox | https://toolbox.googleapps.com/apps/checkmx/ |

---

> Apex Planet Cybersecurity Internship · Gophish v0.12.1 · Kali Linux · March 2026


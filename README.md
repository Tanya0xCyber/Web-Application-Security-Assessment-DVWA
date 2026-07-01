# Web Application Security Assessment – DVWA

## Overview

This repository documents a manual **Web Application Security Assessment** performed on **Damn Vulnerable Web Application (DVWA)** in a controlled lab environment. The assessment demonstrates the identification, exploitation, and documentation of common web application vulnerabilities using industry-standard penetration testing techniques.

---

## Objectives

- Identify common web application vulnerabilities.
- Validate each vulnerability through manual testing.
- Analyze security controls across different security levels.
- Document findings with evidence, impact, and remediation.

---

## Tools Used

- Burp Suite
- Firefox Developer Tools
- Kali Linux
- Nmap
- SQLMap *(Verification Only)*

---

## Vulnerabilities Assessed

- SQL Injection (SQLi)
- Reflected Cross-Site Scripting (XSS)
- Stored Cross-Site Scripting (XSS)
- DOM-Based Cross-Site Scripting (XSS)
- Command Injection
- File Upload
- File Inclusion (LFI)
- Cross-Site Request Forgery (CSRF)
- Brute Force Authentication

---

## Repository Structure

```text
Web-Application-Security-Assessment-DVWA/
│
├── README.md
├── Vulnerabilities/
│        ├── SQL_Injection.md
│        ├── Command_Injection.md
│        ├── File_Upload.md
│        ├── File_Inclusion.md
│        ├── CSRF.md
│        ├── Brute_Force.md
│        ├── XSS/
│            ├── reflected_xss.md
│            ├── stored_xss.md
│            └── dom_xss.md
└── Screenshots/
```

---

## Report Structure

Each vulnerability report includes:

- Overview
- Security Level Comparison
- Attack Flow
- Evidence
- Observation
- Root Cause
- Impact
- Remediation

---

## Disclaimer

This project was conducted in a controlled laboratory environment using **Damn Vulnerable Web Application (DVWA)** for educational and ethical security testing purposes only.

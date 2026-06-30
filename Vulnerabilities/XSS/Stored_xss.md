# Stored Cross-Site Scripting (Stored XSS)

## Overview

Stored Cross-Site Scripting (Stored XSS) occurs when user-supplied input is permanently stored by the application (e.g., in a database) and executed whenever users access the affected page.

## Security Level Comparison

| Feature | Low | Medium | High |
|---------|------|---------|------|
| Protection | No filtering | Partial filtering | Improved filtering |
| Bypass Technique | Standard `<script>` | Mixed-case `<ScRiPt>` *(or standard payload if applicable)* | HTML Event (`onerror`) *(or standard payload if applicable)* |


> **Note:** The tested DVWA version executed `<script>alert(document.domain)</script>` successfully across all security levels. The intended bypass techniques are also included for reference.

---

## Low Security Level

### Testing

| Payload | Purpose | Result |
|---------|---------|--------|
| `<script>alert(document.domain)</script>` | Confirm Stored XSS | JavaScript executed successfully. |
| Refresh Page | Verify persistence | Payload executed again. |

### Observation : 

The application stores user input without validation or output encoding, allowing persistent JavaScript execution.

### Evidence  :

** screenshot - Payload Submission**

```html
<script>alert(document.domain)</script>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Stored_xss/01_low_payload.png" width="100%">
</p>

** screenshot - Stored Payload Execution**

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Stored_xss/02_low_result.png" width="100%">
</p>

---

## Medium Security Level

### Testing

| Payload | Purpose | Result |
|---------|---------|--------|
| `<script>alert(document.domain)</script>` | Verify filtering | JavaScript executed successfully. |
| `<ScRiPt>alert(document.domain)</ScRiPt>` *(Optional)* | Test case-sensitive bypass | Executed successfully. |
| Refresh Page | Verify persistence | Payload remained stored and executed again. |

### Observation

Basic filtering did not prevent persistent JavaScript execution.

### Evidence

 ** Screenshot — Payload Submission ** 

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Stored_xss/03_medium_payload.png" width="100%">
</p>

** Proof of Concept — Stored Payload Execution ** 

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Stored_xss/04_medium_result.png" width="100%">
</p>

---

## High Security Level

### Testing

| Payload | Purpose | Result |
|---------|---------|--------|
| `<script>alert(document.domain)</script>` | Verify filtering | JavaScript executed successfully. |
| `<img src=x onerror=alert(document.domain)>` *(Optional)* | Test HTML event handler | Executed successfully. |
| Refresh Page | Verify persistence | Payload remained stored and executed again. |

### Observation

Despite stronger filtering, persistent JavaScript execution remained possible in the tested environment.

### Evidence

** Screenshot — Payload Submission**

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Stored_xss/05_high_payload.png" width="100%">
</p>

---

** Screenshot— Stored Payload Execution** 

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Stored_xss/06_high_result.png" width="100%">
</p>

---

## Root Cause

The application stores user-controlled input without proper output encoding. Blacklist-based filtering alone is insufficient and can be bypassed.


## Overall Impact

- Execute persistent JavaScript in users' browsers.
- Steal session identifiers or sensitive information.
- Modify webpage content.
- Perform actions as authenticated users.
- Affect every user who visits the vulnerable page.



## Remediation

- Encode user input before rendering it.
- Validate and sanitize user input on the server.
- Avoid blacklist-based filtering.
- Implement a strong Content Security Policy (CSP).
- Protect session cookies using `HttpOnly` and `Secure`.

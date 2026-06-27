# Reflected Cross-Site Scripting (XSS)

## Overview

Reflected Cross-Site Scripting (XSS) occurs when user input is immediately reflected in the application's response without proper sanitization or output encoding, allowing malicious JavaScript to execute in a victim's browser.

---

## Security Level Comparison

| Level      | Protection                                    |
| ---------- | --------------------------------------------- |
| **Low**    | No input filtering                            |
| **Medium** | Basic input filtering                         |
| **High**   | Improved input validation and output encoding |

---

## Low Security

### Testing Approach

* Inject JavaScript payloads into the input field.
* Verify whether the payload is reflected and executed by the browser.

### Payloads

```html
<script>alert('XSS')</script>
```

```html
<img src=x onerror=alert('XSS')>
```

```html
<svg onload=alert('XSS')>
```

### Evidence

**Payload Submitted**

<p align="center">
  <img src="../screenshots/Reflected_xss/01_low_payload.png" width="100%">
</p>

**Successful Script Execution**

<p align="center">
  <img src="../screenshots/Reflected_xss/02_low_result.png" width="100%">
</p>

### Observation

User input was reflected without sanitization, allowing arbitrary JavaScript execution.

---

## Medium Security

### Testing Approach

* Retest the previous payloads.
* Modify payloads to bypass basic filtering.
* Verify whether JavaScript execution is still possible.

### Payloads

```html
(Add successful payload)
```

```html
(Add alternative payload)
```

### Evidence

**Payload Submitted**

<p align="center">
  <img src="../screenshots/Reflected_xss/03_medium_payload.png" width="100%">
</p>

**Successful Filter Bypass**

<p align="center">
  <img src="../screenshots/Reflected_xss/04_medium_result.png" width="100%">
</p>

### Observation

Basic filtering blocked common payloads, but modified payloads successfully bypassed the implemented protection.

---

## High Security

### Testing Approach

* Test advanced payloads against the implemented protections.
* Verify whether JavaScript execution is still possible.

### Payloads

```html
(Add successful payload)
```

```html
(Add alternative payload)
```

### Evidence

**Payload Submitted**

<p align="center">
  <img src="../screenshots/Reflected_xss/05_high_payload.png" width="100%">
</p>

**Execution Result**

<p align="center">
  <img src="../screenshots/Reflected_xss/06_high_result.png" width="100%">
</p>

### Observation

Improved validation and output encoding significantly reduced the effectiveness of common XSS payloads.

---

## Overall Impact

Successful exploitation may allow an attacker to:

* Execute arbitrary JavaScript in a victim's browser.
* Steal session cookies (if not protected).
* Redirect users to malicious websites.
* Capture sensitive user input.
* Perform actions on behalf of authenticated users.

---

## Remediation

* Validate and sanitize all user input.
* Encode output before rendering it in HTML.
* Implement a Content Security Policy (CSP).
* Use **HttpOnly** and **Secure** cookie attributes.
* Avoid inserting untrusted input directly into the DOM.

---


# Stored Cross-Site Scripting (XSS)

## Overview

Stored Cross-Site Scripting (Stored XSS) occurs when user input is permanently stored by the application and later rendered without proper sanitization or output encoding. As a result, malicious JavaScript executes whenever the affected page is viewed.

---

## Security Level Comparison

| Level      | Protection                              | Result                                                     |
| ---------- | --------------------------------------- | ---------------------------------------------------------- |
| **Low**    | No input filtering                      | Payload is stored and executed successfully.               |
| **Medium** | Basic input filtering                   | Common payloads are filtered, requiring bypass techniques. |
| **High**   | Improved validation and output encoding | Payload execution becomes significantly more difficult.    |

---

## Low Security

### Payloads

```html
<script>alert('Stored XSS')</script>
```

```html
<img src=x onerror=alert('Stored XSS')>
```

```html
<svg onload=alert('Stored XSS')>
```

### Evidence

**Payload Submitted**

<p align="center">
  <img src="../screenshots/Stored_xss/01_low_payload.png" width="100%">
</p>

**Stored Payload Executed**

<p align="center">
  <img src="../screenshots/Stored_xss/02_low_result.png" width="100%">
</p>

### Observation

The payload was stored without modification and executed automatically whenever the page was revisited.

---

## Medium Security

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
  <img src="../screenshots/Stored_xss/03_medium_payload.png" width="100%">
</p>

**Stored Payload Executed**

<p align="center">
  <img src="../screenshots/Stored_xss/04_medium_result.png" width="100%">
</p>

### Observation

Basic filtering blocked common payloads, but modified payloads successfully bypassed the implemented protection.

---

## High Security

### Payloads

```html
(Add successful payload if applicable)
```

```html
(Add alternative payload)
```

### Evidence

**Payload Submitted**

<p align="center">
  <img src="../screenshots/Stored_xss/05_high_payload.png" width="100%">
</p>

**Execution Result**

<p align="center">
  <img src="../screenshots/Stored_xss/06_high_result.png" width="100%">
</p>

### Observation

Improved input validation prevented the tested payloads from executing successfully.

---

## Overall Impact

Unlike Reflected XSS, the malicious payload remains stored on the application and executes whenever the affected page is viewed. This increases the likelihood of impacting multiple users.

Successful exploitation may allow an attacker to:

* Execute arbitrary JavaScript in users' browsers.
* Steal session cookies (if not protected).
* Capture sensitive user input.
* Redirect users to malicious websites.
* Perform persistent client-side attacks.

---

## Remediation

* Validate and sanitize user input before storing it.
* Encode user-controlled data before rendering it in HTML.
* Implement a **Content Security Policy (CSP)**.
* Use **HttpOnly** and **Secure** cookie attributes.
* Regularly review and remove malicious user-generated content.

---


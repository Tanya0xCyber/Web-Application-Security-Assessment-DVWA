# Content Security Policy (CSP) Bypass

## Overview

Content Security Policy (CSP) is a browser security feature that helps reduce the impact of Cross-Site Scripting (XSS). A weak or misconfigured policy may still allow malicious JavaScript to execute.

---

## Security Level Comparison

| Level      | Protection          | Result                                                   |
| ---------- | ------------------- | -------------------------------------------------------- |
| **Low**    | Weak or missing CSP | Common payloads execute successfully.                    |
| **Medium** | Basic CSP rules     | Some payloads are blocked, but bypasses remain possible. |
| **High**   | Strict CSP policy   | Tested payloads are blocked.                             |

---

## Low Security

### Test Procedure

1. Inspect the response headers in **Burp Suite** or the browser's **Network** tab.
2. Check whether the `Content-Security-Policy` header is missing or weak.
3. Test common XSS payloads.
4. Verify whether JavaScript executes.

### Test Data

```http
Content-Security-Policy: (Not Present)
```

```html
(Add payload used)
```

### Evidence

**Response Header**

<p align="center">
  <img src="../screenshots/CSP_bypass/01_low_header.png" width="100%">
</p>

**Payload Executed**

<p align="center">
  <img src="../screenshots/CSP_bypass/02_low_result.png" width="100%">
</p>

### Finding

No effective CSP was present, allowing the injected JavaScript to execute successfully.

---

## Medium Security

### Test Procedure

1. Review the `Content-Security-Policy` header.
2. Identify weak directives such as `unsafe-inline` or overly permissive sources.
3. Test payloads against the implemented policy.
4. Verify whether script execution is still possible.

### Test Data

```http
(Add CSP header)
```

```html
(Add payload)
```

### Evidence

**Response Header**

<p align="center">
  <img src="../screenshots/CSP_bypass/03_medium_header.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/CSP_bypass/04_medium_result.png" width="100%">
</p>

### Finding

The application implemented CSP, but weak policy directives allowed the protection to be bypassed.

---

## High Security

### Test Procedure

1. Review the CSP configuration.
2. Confirm that unsafe directives are not present.
3. Test known bypass techniques.
4. Verify whether the browser blocks script execution.

### Test Data

```http
(Add CSP header)
```

```html
(Add payload)
```

### Evidence

**Response Header**

<p align="center">
  <img src="../screenshots/CSP_bypass/05_high_header.png" width="100%">
</p>

**Blocked Payload**

<p align="center">
  <img src="../screenshots/CSP_bypass/06_high_result.png" width="100%">
</p>

### Finding

The implemented CSP correctly prevented the tested payloads from executing.

---

## Overall Impact

If CSP protections are weak or missing, an attacker may:

* Execute malicious JavaScript.
* Bypass XSS protections.
* Steal sensitive client-side data.
* Perform actions as the authenticated user.

---

## Remediation

* Define a strict `Content-Security-Policy` header.
* Avoid `unsafe-inline` and `unsafe-eval`.
* Allow scripts only from trusted sources.
* Regularly review and test the CSP configuration.

---


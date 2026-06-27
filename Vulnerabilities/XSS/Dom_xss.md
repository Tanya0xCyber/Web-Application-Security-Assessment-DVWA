# DOM-Based Cross-Site Scripting (XSS)

## Overview

DOM-Based Cross-Site Scripting (DOM XSS) occurs when client-side JavaScript processes untrusted user input and updates the page without proper validation or encoding. Unlike Reflected or Stored XSS, the payload executes entirely within the browser.

---

## Security Level Comparison

| Level      | Protection                  | Result                                                           |
| ---------- | --------------------------- | ---------------------------------------------------------------- |
| **Low**    | Unsafe DOM manipulation     | Payload executes directly in the browser.                        |
| **Medium** | Basic client-side filtering | Simple payloads are filtered, but bypasses remain possible.      |
| **High**   | Improved input validation   | Common payloads are blocked, making exploitation more difficult. |

---

## Low Security

### Payloads

```html id="x9g2tv"
#<script>alert('DOM XSS')</script>
```

```html id="7s0q5p"
#<img src=x onerror=alert('DOM XSS')>
```

```html id="dvwvsl"
#<svg onload=alert('DOM XSS')>
```

### Evidence

**Input**

<p align="center">
  <img src="../screenshots/DOM_xss/01_low_payload.png" width="100%">
</p>

**Result**

<p align="center">
  <img src="../screenshots/DOM_xss/02_low_result.png" width="100%">
</p>

### Observation

The client-side script processed untrusted input and executed it directly in the browser.

---

## Medium Security

### Payloads

```html id="5l9fiz"
(Add successful payload)
```

```html id="1kgw7h"
(Add alternative payload)
```

### Evidence

**Input**

<p align="center">
  <img src="../screenshots/DOM_xss/03_medium_payload.png" width="100%">
</p>

**Result**

<p align="center">
  <img src="../screenshots/DOM_xss/04_medium_result.png" width="100%">
</p>

### Observation

Basic client-side filtering blocked common payloads, but modified payloads remained effective.

---

## High Security

### Payloads

```html id="jlwmk2"
(Add successful payload if applicable)
```

```html id="jjvca9"
(Add alternative payload)
```

### Evidence

**Input**

<p align="center">
  <img src="../screenshots/DOM_xss/05_high_payload.png" width="100%">
</p>

**Result**

<p align="center">
  <img src="../screenshots/DOM_xss/06_high_result.png" width="100%">
</p>

### Observation

Improved client-side validation reduced the effectiveness of the tested payloads.

---

## Overall Impact

Successful exploitation may allow an attacker to:

* Execute JavaScript in the victim's browser.
* Steal sensitive information displayed on the page.
* Redirect users to malicious websites.
* Manipulate page content.
* Perform actions using the victim's session.

---

## Remediation

* Never insert untrusted data directly into the DOM.
* Use safe APIs such as `textContent` instead of `innerHTML` whenever possible.
* Validate and encode user input before using it in JavaScript.
* Implement a Content Security Policy (CSP).

---

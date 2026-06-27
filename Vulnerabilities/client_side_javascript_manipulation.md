# Client-Side JavaScript Manipulation

## Overview

Some applications rely on JavaScript in the browser to validate user input. Since client-side code is fully controlled by the user, these checks can be modified or bypassed if the server does not perform its own validation.

---

## Security Level Comparison

| Level      | Protection                    | Result                              |
| ---------- | ----------------------------- | ----------------------------------- |
| **Low**    | Client-side validation only   | Validation can be bypassed.         |
| **Medium** | Additional client-side checks | Bypass requires extra modification. |
| **High**   | Server-side validation        | Modified requests are rejected.     |

---

## Low Security

### Test Procedure

1. Open the page and press **F12** to inspect the JavaScript.
2. Identify input validation performed in the browser (e.g., required fields or value checks).
3. Disable or modify the validation using **Developer Tools**, or intercept the request with **Burp Suite**.
4. Submit the modified request and verify whether the server accepts it.

### Test Data

```javascript
(Add modified validation or request)
```

### Evidence

**Client-Side Validation**

<p align="center">
  <img src="../screenshots/JavaScript/01_low_script.png" width="100%">
</p>

**Modified Request Accepted**

<p align="center">
  <img src="../screenshots/JavaScript/02_low_result.png" width="100%">
</p>

### Finding

The application trusted client-side validation, allowing modified requests to bypass the intended restrictions.

---

## Medium Security

### Test Procedure

1. Review the updated JavaScript validation.
2. Identify additional checks implemented in the browser.
3. Modify the validation or the intercepted request.
4. Verify whether the server still accepts the modified data.

### Test Data

```javascript
(Add modified validation)
```

### Evidence

**JavaScript Validation**

<p align="center">
  <img src="../screenshots/JavaScript/03_medium_script.png" width="100%">
</p>

**Modified Request**

<p align="center">
  <img src="../screenshots/JavaScript/04_medium_result.png" width="100%">
</p>

### Finding

Additional client-side checks increased the effort required, but validation could still be bypassed.

---

## High Security

### Test Procedure

1. Inspect the client-side validation.
2. Modify the request using **Burp Suite**.
3. Submit the altered request.
4. Verify whether the server independently validates the input.

### Test Data

```http
(Add modified request)
```

### Evidence

**Modified Request**

<p align="center">
  <img src="../screenshots/JavaScript/05_high_script.png" width="100%">
</p>

**Server Response**

<p align="center">
  <img src="../screenshots/JavaScript/06_high_result.png" width="100%">
</p>

### Finding

Although client-side validation could be modified, the server rejected invalid input, preventing the bypass.

---

## Overall Impact

If an application relies only on client-side validation, an attacker may:

* Bypass input restrictions.
* Submit unauthorized values.
* Manipulate application behavior.
* Circumvent business logic.

---

## Remediation

* Always validate input on the server.
* Treat client-side validation as a usability feature, not a security control.
* Verify sensitive actions before processing requests.
* Reject modified or unexpected input.

---


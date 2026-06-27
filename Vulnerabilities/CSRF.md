# Cross-Site Request Forgery (CSRF)

## Overview

Cross-Site Request Forgery (CSRF) allows an attacker to perform actions on behalf of an authenticated user by sending a forged request that the application trusts.

---

## Security Level Comparison

| Level      | Protection                 | Result                              |
| ---------- | -------------------------- | ----------------------------------- |
| **Low**    | No CSRF token              | Forged requests are accepted.       |
| **Medium** | Partial request validation | Basic attacks become less reliable. |
| **High**   | CSRF token validation      | Unauthorized requests are rejected. |

---

## Low Security

### Verification

A forged request was created to perform a password change without requiring additional user interaction.

### Attack Input

```html
(Add forged HTML request)
```

### Evidence

**Forged Request**

<p align="center">
  <img src="../screenshots/CSRF/01_low_request.png" width="100%">
</p>

**Password Updated**

<p align="center">
  <img src="../screenshots/CSRF/02_low_result.png" width="100%">
</p>

### Finding

The application processed the forged request because no mechanism was present to verify that the request originated from the legitimate user.

---

## Medium Security

### Verification

The same attack was repeated after introducing additional request validation.

### Attack Input

```html
(Add forged request)
```

### Evidence

**Forged Request**

<p align="center">
  <img src="../screenshots/CSRF/03_medium_request.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/CSRF/04_medium_result.png" width="100%">
</p>

### Finding

Basic validation reduced the success rate of the attack, although bypasses remained possible.

---

## High Security

### Verification

The attack was repeated against the hardened implementation.

### Attack Input

```html
(Add forged request)
```

### Evidence

**Forged Request**

<p align="center">
  <img src="../screenshots/CSRF/05_high_request.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/CSRF/06_high_result.png" width="100%">
</p>

### Finding

Requests without a valid CSRF token were rejected, preventing unauthorized actions.

---

## Overall Impact

If exploited successfully, an attacker may be able to:

* Change account information.
* Reset passwords or email addresses.
* Trigger sensitive actions using the victim's session.
* Perform unauthorized operations without the victim's knowledge.

---

## Remediation

* Require a unique **CSRF token** for every sensitive request.
* Validate the **Origin** or **Referer** header when appropriate.
* Use **SameSite** cookies to reduce cross-site requests.
* Require password confirmation for critical account changes.

---


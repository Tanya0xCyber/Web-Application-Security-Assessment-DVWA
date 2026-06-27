# Weak Session IDs

## Overview

Weak Session IDs occur when an application generates predictable session identifiers. Attackers may guess or predict valid session IDs and hijack active user sessions.

---

## Security Level Comparison

| Level      | Protection                | Result                                                 |
| ---------- | ------------------------- | ------------------------------------------------------ |
| **Low**    | Sequential session IDs    | Session IDs are easily predictable.                    |
| **Medium** | Slightly randomized IDs   | Prediction becomes more difficult but patterns remain. |
| **High**   | Secure random session IDs | Session prediction is no longer practical.             |

---

## Low Security

### Test Procedure

1. Generate multiple sessions.
2. Compare the issued session IDs.
3. Check whether the values follow a predictable pattern.

### Test Data

```text
Session ID 1
Session ID 2
Session ID 3
```

### Evidence

**Generated Session IDs**

<p align="center">
  <img src="../screenshots/Weak_session_ids/01_low_ids.png" width="100%">
</p>

**Pattern Identified**

<p align="center">
  <img src="../screenshots/Weak_session_ids/02_low_result.png" width="100%">
</p>

### Finding

The application generated predictable session IDs, making active sessions easier to guess.

---

## Medium Security

### Test Procedure

1. Generate multiple session IDs.
2. Compare the values for predictable patterns.
3. Assess whether the added randomness prevents prediction.

### Test Data

```text
(Add generated session IDs)
```

### Evidence

**Generated Session IDs**

<p align="center">
  <img src="../screenshots/Weak_session_ids/03_medium_ids.png" width="100%">
</p>

**Pattern Analysis**

<p align="center">
  <img src="../screenshots/Weak_session_ids/04_medium_result.png" width="100%">
</p>

### Finding

Additional randomness reduced predictability, although identifiable patterns remained.

---

## High Security

### Test Procedure

1. Generate multiple session IDs.
2. Compare the values for repeating or predictable patterns.
3. Verify whether session IDs appear securely randomized.

### Test Data

```text
(Add generated session IDs)
```

### Evidence

**Generated Session IDs**

<p align="center">
  <img src="../screenshots/Weak_session_ids/05_high_ids.png" width="100%">
</p>

**Pattern Analysis**

<p align="center">
  <img src="../screenshots/Weak_session_ids/06_high_result.png" width="100%">
</p>

### Finding

Session IDs appeared sufficiently random, making prediction impractical.

---

## Overall Impact

Successful exploitation may allow an attacker to:

* Hijack active user sessions.
* Bypass authentication.
* Access another user's account.
* Perform actions as the victim.

---

## Remediation

* Generate session IDs using a cryptographically secure random number generator.
* Regenerate the session ID after login and privilege changes.
* Expire inactive sessions and invalidate them after logout.
* Mark session cookies as **HttpOnly**, **Secure**, and **SameSite**.

---


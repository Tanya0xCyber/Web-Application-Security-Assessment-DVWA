# Cross-Site Request Forgery (CSRF)

## Overview

Cross-Site Request Forgery (CSRF) is a vulnerability that forces an authenticated user to perform unwanted actions without their knowledge by sending forged requests to the application.

## Security Level Comparison

| Feature | Low | Medium |
|---------|------|---------|
| Protection | None | Referer Header Validation |
| Successful Attack | Direct URL | Valid Referer Required | 

> **Note:** Testing was performed on the DVWA (Linux) environment.

## Low Security Level

### Attack Flow

| Step | Action | Result |
|------|--------|--------|
| 1 | Change password normally | Password updated successfully. |
| 2 | Copy the generated URL | Request captured. |
| 3 | Open the same URL while authenticated | ✅ Password changed again without confirmation. |

### Request

```text
?password_new=test123&password_conf=test123&Change=Change
```

### Observation

The application accepts password change requests without verifying where the request originated.

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/CSRF/01_low_result.png" width="100%">
</p>

---

## Medium Security Level

### Attack Flow

| Test | Result |
|------|--------|
| Remove `Referer` header | ❌ Request rejected |
| Fake `Referer` header | ❌ Request rejected |
| Valid DVWA `Referer` header | ✅ Password changed |

### Observation

The application verifies the `Referer` header before processing the request. Requests without the expected `Referer` are rejected.

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/CSRF/02_medium_result.png" width="100%">
</p>

---

## Root Cause

User requests are not consistently protected against unauthorized actions. Missing or weak request validation allows attackers to perform actions on behalf of authenticated users.

## Overall Impact

- Unauthorized password changes.
- Account compromise.
- Unauthorized actions performed as authenticated users.
- Complete application compromise if administrative accounts are targeted.

## Remediation

- Use anti-CSRF tokens for all state-changing requests.
- Verify the `Origin` or `Referer` header where appropriate.
- Use the `SameSite` attribute for session cookies.
- Require user re-authentication for sensitive actions.
- Combine CSRF protection with secure session management.

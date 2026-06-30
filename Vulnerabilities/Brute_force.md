# Brute Force Authentication

## Overview

Brute Force is an authentication attack where multiple username and password combinations are tested until valid credentials are identified.

## Security Level Comparison

| Level      | Protection                          | Result                                          |
| ---------- | ----------------------------------- | ----------------------------------------------- |
| **Low**    | No rate limiting or account lockout | Automated password guessing succeeds.           |
| **Medium** | Basic authentication controls       | Attack becomes slower but remains possible.     |
| **High**   | Stronger authentication controls    | Automated attacks are significantly restricted. |

---

## Low Security

### Attack Flow

| Action | Purpose | Result |
|--------|---------|--------|
| Valid Login (`admin:password`) | Verify functionality | Login successful. |
| Invalid Credentials | Check authentication | Login failed. |
| Multiple Login Attempts | Test brute force protection | Unlimited attempts allowed. |
| Burp Suite Intruder *(Optional)* | Automate login attempts | Requests processed without restriction. |

### Evidence

**Intruder Configuration**

<p align="center">
  <img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/Brute_force/01_low_attempt.png" width="100%">
</p>

**Successful Authentication**

<p align="center">
  <img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/Brute_force/02_low_result.png" width="100%">
</p>

### Finding

Unlimited login attempts allowed valid credentials to be identified through response analysis.

---

## Medium Security

###  Attack Flow

| Action | Purpose | Result |
|--------|---------|--------|
| Invalid Credentials | Verify protection | 2-second delay introduced. |
| Multiple Login Attempts | Test brute force resistance | Requests remain unlimited but slower. |
| Burp Suite Intruder *(Optional)* | Automate login attempts | Attack still possible with reduced speed. |


### Finding

Basic protections slowed automated attacks but did not prevent credential guessing.

---

## High Security

### Attack Flow

| Action | Purpose | Result |
|--------|---------|--------|
| Inspect Request | Identify security controls | CSRF token detected. |
| Invalid Credentials | Verify protection | Random delay (2–4 seconds). |
| Valid CSRF Token + Login Attempt | Test brute force feasibility | Brute force remains possible with valid tokens. |


### Evidence

<p align="center">
  <img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/Brute_force/03_high_csrf.png" width="100%">
</p>

### Finding

Improved authentication controls significantly reduced the effectiveness of brute force attacks.

---

## Overall Impact

Successful exploitation may allow an attacker to:

* Gain unauthorized access to user accounts.
* Compromise sensitive information.
* Escalate privileges through compromised accounts.
* Reuse valid credentials across other services.

---

## Remediation

* Limit repeated login attempts using rate limiting or account lockout.
* Enforce strong password policies.
* Enable Multi-Factor Authentication (MFA).
* Monitor and alert on repeated failed login attempts.

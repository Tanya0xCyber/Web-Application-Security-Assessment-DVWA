# Brute Force Authentication

## Overview

Brute Force is an authentication attack where multiple username and password combinations are tested until valid credentials are discovered. The effectiveness of the attack depends on the application's authentication controls.

---

## Security Level Comparison

| Level      | Protection                          | Result                                          |
| ---------- | ----------------------------------- | ----------------------------------------------- |
| **Low**    | No rate limiting or account lockout | Automated password guessing succeeds.           |
| **Medium** | Basic authentication controls       | Attack becomes slower but remains possible.     |
| **High**   | Stronger authentication protection  | Automated attacks are significantly restricted. |

---

## Low Security

### Test Procedure

1. Intercept the login request using **Burp Suite**.
2. Send the request to **Intruder**.
3. Configure the password parameter as the payload position (**Sniper Attack**).
4. Load a password wordlist and start the attack.
5. Compare the responses to identify successful authentication.

### Attack Method

* **Tool:** Burp Suite Intruder
* **Attack Type:** Sniper
* **Payload:** Password Wordlist

### Evidence

**Intruder Configuration**

<p align="center">
  <img src="../screenshots/Brute_force/01_low_intruder.png" width="100%">
</p>

**Successful Login Response**

<p align="center">
  <img src="../screenshots/Brute_force/02_low_result.png" width="100%">
</p>

### Finding

The application allowed unlimited authentication attempts without implementing account lockout or request throttling. The valid password was identified by observing a different response length and successful login message.

---

## Medium Security

### Test Procedure

1. Repeat the attack using the same Intruder configuration.
2. Observe additional security controls such as request delays.
3. Compare the application responses to determine whether valid credentials can still be identified.

### Attack Method

* **Tool:** Burp Suite Intruder
* **Attack Type:** Sniper
* **Payload:** Password Wordlist

### Evidence

**Intruder Results**

<p align="center">
  <img src="../screenshots/Brute_force/03_medium_intruder.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/Brute_force/04_medium_result.png" width="100%">
</p>

### Finding

Additional authentication controls reduced the speed of the attack but did not completely prevent password guessing.

---

## High Security

### Test Procedure

1. Repeat the automated attack.
2. Monitor the responses for lockout mechanisms, delays, or failed authentication.
3. Verify whether valid credentials can still be identified.

### Attack Method

* **Tool:** Burp Suite Intruder
* **Attack Type:** Sniper
* **Payload:** Password Wordlist

### Evidence

**Intruder Results**

<p align="center">
  <img src="../screenshots/Brute_force/05_high_intruder.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/Brute_force/06_high_result.png" width="100%">
</p>

### Finding

Improved authentication controls significantly reduced the effectiveness of automated brute force attacks.

---

## Overall Impact

Successful exploitation may allow an attacker to:

* Gain unauthorized access to user accounts.
* Compromise sensitive information.
* Escalate privileges through compromised administrative accounts.
* Reuse discovered credentials across other services.

---

## Remediation

* Limit repeated login attempts using rate limiting or account lockout.
* Enforce strong password policies.
* Implement Multi-Factor Authentication (MFA).
* Monitor and alert on repeated failed login attempts.


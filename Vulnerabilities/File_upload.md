# Unrestricted File Upload

## Overview

Unrestricted File Upload allows attackers to upload malicious files to the server. If these files are executed, they can lead to remote code execution (RCE) and complete system compromise.

## Security Level Comparison

| Feature | Low | Medium | High |
|---------|------|---------|------|
| Protection | None | Client-side MIME type validation | Image processing after upload |
| Bypass Technique | Upload PHP file | Modify MIME type | Requires another vulnerability (e.g., File Inclusion) |
| Code Execution | ✅ | ✅ | Depends on application |

> **Note:** Results are based on the tested DVWA environment.

---


## Low Security Level

3## Attack Flow

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

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/File_upload/01_low_result.png" width="100%">
</p>

---

## Medium Security Level

### Attack Flow

| Step | Action | Result |
|------|--------|--------|
| 1 | Upload `shell.php` | ❌ Rejected |
| 2 | Intercept request using Burp Suite | Request captured |
| 3 | Change `Content-Type` to `image/jpeg` | Upload accepted |
| 4 | Browse to uploaded file | PHP executed successfully |

### Observation

The application trusts the client-supplied MIME type. Modifying the `Content-Type` header bypasses the upload restriction.

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/File_upload/02_medium_result.png" width="100%">
</p>

---

## High Security Level

###  Attack flow

| Step | Action | Result |
|------|--------|--------|
| 1 | Upload `shell.php` | ❌ Rejected (invalid extension). |
| 2 | Change MIME type using Burp Suite | ❌ Rejected. |
| 3 | Rename file (`shell.php.png`) | ❌ Rejected (`getimagesize()` validation failed). |
| 4 | Create a polyglot image containing PHP code | ✅ Upload accepted. |
| 5 | Access uploaded image directly | ❌ PHP not executed. |
| 6 | Chain with another vulnerability (e.g., LFI) | ✅ PHP code can be executed if the uploaded file is included by the server. |

### Polyglot Payload Creation

Create a PHP payload:

```php
<?php
phpinfo();
?>
```

Append it to a valid image:

```bash
cat image.jpg shell.php > image_shell.jpg
```

Verify the file is still recognized as an image:

```bash
file image_shell.jpg
```

Expected Output

```text
JPEG image data
```

### Observation

The application validates the file extension, file size, and image content using `getimagesize()`. A polyglot image bypasses the upload validation because it remains a valid image, but direct code execution is prevented. Exploitation requires chaining with another vulnerability, such as Local File Inclusion (LFI).

### Evidence

<p align="center">
<img src="https://github.com/Tanya0xCyber/Web-Application-Security-Assessment-DVWA/blob/main/Screenshots/File_upload/03_high_result.png" width="100%">
</p>

---

## Root Cause

The application allows user-controlled files to be uploaded without implementing secure server-side validation or isolating uploaded files from executable locations.

## Overall Impact

- Remote Code Execution (RCE).
- Upload of web shells or backdoors.
- Unauthorized access to server resources.
- Data theft or modification.
- Complete server compromise if combined with another vulnerability.

## Remediation

- Allow only required file extensions using a strict allowlist.
- Validate the actual file content on the server.
- Store uploaded files outside the web root.
- Rename uploaded files using random filenames.
- Disable script execution in upload directories.
- Perform malware scanning before storing uploaded files.

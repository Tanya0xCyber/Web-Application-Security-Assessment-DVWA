# SQL Injection (SQLi)

## Overview

SQL Injection (SQLi) is a vulnerability where unsanitized user input is interpreted as part of a SQL query, allowing attackers to access, modify, or delete database information.

---

## Assessment Objective

Evaluate the SQL Injection module in DVWA across different security levels to understand how security controls influence exploitation and how each level can be tested and documented.

---

## Security Level Comparison

| Level      | Security Controls                          | Observation                                               |
| ---------- | ------------------------------------------ | --------------------------------------------------------- |
| **Low**    | No input validation                        | Basic payloads execute successfully.                      |
| **Medium** | Basic input filtering                      | Payloads require minor modifications to bypass filtering. |
| **High**   | Improved validation and restricted queries | Exploitation becomes significantly more difficult.        |

---

# Low Security Level

### Exploitation Process

1. Set DVWA security to **Low**.
2. Verify the normal response using a valid ID.
3. Inject SQL payloads into the input field.
4. Confirm whether unauthorized records are returned.

### Payloads

```sql
1' OR '1'='1
```

```sql
1' UNION SELECT user, password FROM users#
```

```sql
' OR 1=1#
```

### Proof of Concept

**Payload Execution**

<p align="center">
  <img src="screenshots/SQLi/01_low_payload.png" width="100%">
</p>

**Successful Result**

<p align="center">
  <img src="screenshots/SQLi/02_low_result.png" width="100%">
</p>

**Observation:** The application directly concatenates user input into SQL queries, making exploitation straightforward.

---

# Medium Security Level

### Exploitation Process

1. Switch DVWA to **Medium** security.
2. Retest the previous payloads.
3. Modify payloads where necessary to bypass input filtering.
4. Verify whether database information is still disclosed.

### Payloads

```sql
(Add successful payload)
```

```sql
(Add alternative payload)
```

### Proof of Concept

**Payload Execution**

<p align="center">
  <img src="screenshots/SQLi/03_medium_payload.png" width="100%">
</p>

**Successful Result**

<p align="center">
  <img src="screenshots/SQLi/04_medium_result.png" width="100%">
</p>

**Observation:** Basic filtering blocks some payloads, but the application remains vulnerable after adjusting the injection technique.

---

# High Security Level

### Exploitation Process

1. Set DVWA security to **High**.
2. Analyze the implemented security controls.
3. Test advanced payloads against the input field.
4. Record whether SQL Injection remains exploitable.

### Payloads

```sql
(Add successful payload)
```

```sql
(Add alternative payload)
```

### Proof of Concept

**Payload Execution**

<p align="center">
  <img src="screenshots/SQLi/05_high_payload.png" width="100%">
</p>

**Successful Result**

<p align="center">
  <img src="screenshots/SQLi/06_high_result.png" width="100%">
</p>

**Observation:** Stronger input validation significantly reduces the effectiveness of common SQL Injection payloads.

---

# Overall Impact

If successfully exploited, SQL Injection may allow an attacker to:

* Access sensitive database information.
* Bypass authentication.
* Enumerate database tables and columns.
* Modify or delete stored data.
* Gain complete database compromise if excessive privileges exist.

---

# Remediation

* Use **prepared statements (parameterized queries)** instead of dynamic SQL.
* Validate and sanitize user input using allowlists.
* Apply the **principle of least privilege** to database accounts.
* Disable verbose database error messages in production.
* Perform regular security testing before deployment.

---



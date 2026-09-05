# SQL Injection Notes

## Task Information

**Task:** SQL Injection on DVWA (Low Security)

**Objective:** Demonstrate a classic SQL Injection vulnerability, document the attack process, analyze the results, and explain mitigation techniques.

**Environment:**

- Operating System: Windows
- Web Server: Apache (XAMPP)
- Database: MySQL/MariaDB
- Application: DVWA (Damn Vulnerable Web Application)
- Security Level: Low

---

# Initial Setup Verification

## DVWA Configuration

DVWA was installed locally using XAMPP.

Configuration file:

```text
C:\xampp\htdocs\DVWA-master\config\config.inc.php
```

Database settings:

```php
$_DVWA['db_server'] = '127.0.0.1';
$_DVWA['db_database'] = 'dvwa';
$_DVWA['db_user'] = 'root';
$_DVWA['db_password'] = '';
$_DVWA['db_port'] = '3306';
```

Database was successfully created using:

```text
http://127.0.0.1/DVWA-master/setup.php
```

Default credentials:

```text
Username: admin
Password: password
```

---

# Vulnerability Overview

SQL Injection occurs when user-supplied input is inserted directly into an SQL query without proper validation or parameterized execution.

An attacker can manipulate the query logic to:

- Retrieve unauthorized information
- Bypass authentication
- Enumerate users
- Modify database contents
- Access sensitive records

---

# Baseline Testing

## Test 1 – Normal Input

### Payload

```text
1
```

### Purpose

Verify normal application behavior before attempting SQL Injection.

### Result

```text
ID: 1
First name: admin
Surname: admin
```

### Analysis

The application returned only the record associated with User ID 1.

This confirms that:

- The SQL Injection module is functioning.
- Database connectivity is working.
- The application is processing input normally.

### Likely Query

```sql
SELECT first_name, last_name
FROM users
WHERE user_id = '1';
```

---

# SQL Injection Attempt 1

## Payload

```text
1' OR '1'='1
```

## Purpose

Attempt to alter the query logic so that the WHERE clause always evaluates to TRUE.

## Result

The application returned multiple records:

```text
admin
Gordon Brown
Hack Me
Pablo Picasso
Bob Smith
```

## Screenshot

```text
screenshots/sql_payload_1.png
```

## Analysis

The payload injected an additional condition:

```sql
OR '1'='1'
```

Since:

```sql
'1'='1'
```

always evaluates to TRUE, the query returns every row from the users table.

### Expected Query Structure

Original:

```sql
SELECT first_name, last_name
FROM users
WHERE user_id = '1';
```

Injected:

```sql
SELECT first_name, last_name
FROM users
WHERE user_id = '1'
OR '1'='1';
```

### Impact

- Unauthorized disclosure of user data
- User enumeration
- Exposure of database contents

### Status

```text
SUCCESSFUL
```

---

# SQL Injection Attempt 2

## Payload

```text
1' OR '1'='1' #
```

## Purpose

Use a MySQL comment character to ignore the remainder of the SQL query.

## Result

Returned multiple user records.

## Screenshot

```text
screenshots/sql_payload_2.png
```

## Analysis

The hash symbol:

```sql
#
```

acts as a comment in MySQL.

Everything following the comment is ignored by the database engine.

This allows the attacker to terminate the intended query structure and control execution flow.

### Example

```sql
SELECT first_name, last_name
FROM users
WHERE user_id = '1'
OR '1'='1' #';
```

The database ignores the remaining characters after the comment marker.

### Impact

- Unauthorized data access
- Query manipulation
- Information disclosure

### Status

```text
SUCCESSFUL
```

---

# Error-Based Testing

## Payload

```text
'1'='1
```

## Result

Application generated a SQL syntax error.

### Error Observed

```text
You have an error in your SQL syntax
```

### Analysis

The payload did not properly modify the SQL statement structure.

Although the injection failed, the detailed database error message revealed:

- Database technology in use
- Query processing behavior
- Potential attack surface

### Security Concern

Displaying raw database errors helps attackers understand the backend system.

### Status

```text
FAILED INJECTION
INFORMATION DISCLOSURE PRESENT
```

---

# Security Risks Identified

## 1. Information Disclosure

Sensitive user information can be retrieved without authorization.

## 2. User Enumeration

Attackers can discover valid user accounts.

## 3. Authentication Bypass Potential

Similar payloads can be used against login forms.

## 4. Database Exposure

Poor query construction exposes backend data.

## 5. Detailed Error Messages

Database errors provide attackers with useful technical information.

---

# Mitigation Techniques

## Prepared Statements

Use parameterized queries instead of directly concatenating user input.

Example:

```php
$stmt = $pdo->prepare(
    "SELECT first_name, last_name
     FROM users
     WHERE user_id = ?"
);

$stmt->execute([$id]);
```

## Input Validation

Validate all user-supplied input.

## Output Sanitization

Prevent unintended data exposure.

## Least Privilege

Database accounts should only have required permissions.

## Secure Error Handling

Display generic error messages to users while logging technical details internally.

## Security Testing

Perform regular vulnerability assessments and code reviews.

---

# Lessons Learned

- SQL Injection is caused by unsafe query construction.
- Even simple payloads can expose sensitive information.
- Error messages can reveal valuable information to attackers.
- Prepared statements effectively prevent SQL Injection.
- Security testing should always be performed in controlled environments such as DVWA.

---

# Conclusion

The DVWA SQL Injection lab successfully demonstrated how insecure SQL queries can be exploited to retrieve unauthorized information. Multiple payloads were tested, resulting in disclosure of user records from the backend database. The exercise reinforced the importance of prepared statements, input validation, secure coding practices, and proper error handling in web applications.

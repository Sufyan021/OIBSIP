# SQL Injection on DVWA (Low Security)

## Overview

This project demonstrates a classic SQL Injection vulnerability using DVWA (Damn Vulnerable Web Application) running on a local XAMPP environment. The objective was to understand how SQL Injection works, observe the impact of insecure database queries, and learn how developers can prevent such vulnerabilities.

> Ethical Notice: This activity was performed only on a locally hosted DVWA installation for educational purposes.

---

## Objective

- Install and configure DVWA on a local machine.
- Set DVWA Security Level to Low.
- Navigate to the SQL Injection module.
- Perform SQL Injection attacks using test payloads.
- Analyze the results.
- Document the vulnerability and prevention methods.

---

## Tools Used

- DVWA (Damn Vulnerable Web Application)
- XAMPP
- Apache Web Server
- MySQL Database
- Web Browser

---

## Environment Setup

### 1. Install XAMPP

Downloaded and installed XAMPP.

### 2. Start Services

Started:

- Apache
- MySQL

### 3. Install DVWA

Extracted DVWA into:

```text
C:\xampp\htdocs\DVWA-master
```

### 4. Configure Database

Created:

```text
config/config.inc.php
```

Updated database settings:

```php
$_DVWA['db_server'] = '127.0.0.1';
$_DVWA['db_database'] = 'dvwa';
$_DVWA['db_user'] = 'root';
$_DVWA['db_password'] = '';
$_DVWA['db_port'] = '3306';
```

### 5. Create Database

Opened:

```text
http://127.0.0.1/DVWA-master/setup.php
```

Clicked:

```text
Create / Reset Database
```

### 6. Login

Credentials:

```text
Username: admin
Password: password
```

### 7. Set Security Level

Navigated to:

```text
DVWA Security
```

Selected:

```text
Low
```

---

# Understanding SQL Injection

SQL Injection is a web security vulnerability that occurs when user input is directly inserted into an SQL query without proper validation or parameterization.

An attacker can manipulate the query to:

- View unauthorized data
- Bypass authentication
- Modify records
- Delete records
- Gain access to sensitive information

---

## Normal Application Behavior

Input:

```text
1
```

Output:

```text
ID: 1
First name: admin
Surname: admin
```

The application returned only the record associated with User ID 1.

---

# SQL Injection Attack

## Payload 1

Input:

```text
1' OR '1'='1
```

### Result

The application returned multiple records:

```text
admin
Gordon Brown
Hack Me
Pablo Picasso
Bob Smith
```

### Explanation

The original query is similar to:

```sql
SELECT first_name, last_name
FROM users
WHERE user_id = '1';
```

After injection:

```sql
SELECT first_name, last_name
FROM users
WHERE user_id = '1'
OR '1'='1';
```

Since:

```sql
'1'='1'
```

is always true, the database returns all rows.

### Impact

- Unauthorized data exposure
- Information disclosure
- User enumeration

---

## Payload 2

Input:

```text
1' OR '1'='1' #
```

### Result

Returned multiple user records.

### Explanation

The hash symbol (`#`) acts as a comment character in MySQL.

Everything after it is ignored, helping attackers manipulate query execution.

### Impact

- Bypass intended query logic
- Access data without authorization

---

# Why the Vulnerability Exists

The vulnerability exists because user input is directly concatenated into SQL queries.

Example of vulnerable code:

```php
$query = "SELECT * FROM users WHERE user_id = '$id'";
```

The application trusts user input and sends it directly to the database.

---

# Prevention

Developers can prevent SQL Injection by:

### 1. Prepared Statements

Use parameterized queries instead of string concatenation.

Example:

```php
$stmt = $pdo->prepare(
    "SELECT * FROM users WHERE user_id = ?"
);
$stmt->execute([$id]);
```

### 2. Input Validation

Validate and sanitize user input before processing.

### 3. Least Privilege

Database accounts should have only the permissions they require.

### 4. Error Handling

Do not display database errors to users.

### 5. Web Application Firewalls

Deploy security controls to detect malicious requests.

---

# Screenshots

The following screenshots are included in the screenshots folder:

1. DVWA Setup Page
2. DVWA Security Level (Low)
3. Normal Input Result
4. SQL Injection Payload 1 Result
5. SQL Injection Payload 2 Result

---

# Key Learning Outcomes

- Learned how SQL Injection works.
- Understood the risks of insecure SQL queries.
- Demonstrated data exposure through SQL Injection.
- Learned how prepared statements prevent attacks.
- Gained hands-on experience with DVWA.

---

## Conclusion

This project successfully demonstrated a classic SQL Injection vulnerability on DVWA running in Low Security mode. By injecting crafted SQL payloads into the input field, it was possible to retrieve data that should not have been accessible. The exercise highlights the importance of secure coding practices such as prepared statements, parameterized queries, and proper input validation to protect web applications from SQL Injection attacks.

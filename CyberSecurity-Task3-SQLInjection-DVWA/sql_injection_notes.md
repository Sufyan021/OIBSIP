# SQL Injection Notes

## Environment
- DVWA running on XAMPP
- Security Level: Low

## Normal Input

Payload:
1

Result:
Returned the record for user ID 1 (admin).

## Payload 1

Payload:
1' OR '1'='1

Result:
[Write what DVWA displayed]

Explanation:
The injected condition evaluates to true and alters the SQL query logic.

## Payload 2

Payload:
1' OR '1'='1' #

Result:
[Write what DVWA displayed]

Explanation:
The comment character prevents the rest of the original query from being processed.

## Impact

- Unauthorized data disclosure
- User enumeration
- Authentication bypass risks

## Prevention

- Use prepared statements
- Use parameterized queries
- Validate and sanitize input
- Apply least-privilege database permissions

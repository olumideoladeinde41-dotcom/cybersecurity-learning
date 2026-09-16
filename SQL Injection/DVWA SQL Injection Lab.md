# DVWA SQL Injection Lab — Low Security

**Lab date:** 16 September 2026  
**Status:** SQL injection demonstrated in an intentionally vulnerable training application; remediation and retest not yet performed.

## Purpose and scope

Practice identifying and validating SQL injection in an **isolated, authorized home lab**, not a production system. The test used Ubuntu in VirtualBox to access DVWA hosted on Metasploitable2 over a host-only network (`192.168.56.102`). The application used HTTP; the browser's “Not Secure” indicator reflects the lack of HTTPS and was not itself evidence of SQL injection.

## Baseline and troubleshooting

- On DVWA's **SQL Injection** page, entering User ID `1` returned `admin / admin`.
- User ID `2` returned `Gordon / Brown`.
- User ID `9999` returned no record. A blank result alone therefore did **not** establish a SQL error or successful injection.
- Initial injection attempts returned no visible result. Firefox Developer Tools showed two `security` cookies with different paths and values (`high` on a more specific `/dvwa/v…` path, `low` on `/dvwa`). Removing **only** the conflicting high-security cookie and verifying the SQL Injection page's footer showed `Security Level: low` corrected the test conditions. The session cookie was left untouched.

## Finding: user input alters SQL query logic

**Affected functionality:** DVWA → SQL Injection → User ID field, with DVWA security level set to Low.

The application's source displayed a query in this form:

```sql
SELECT first_name, last_name FROM users WHERE user_id = '$id';
```

The application interpolates the supplied ID into SQL text. A normal input of `1` returned one record (`admin / admin`). An input of `1'` produced a visible MySQL syntax error, consistent with the extra quote disrupting the SQL statement.

A further controlled test used the following input **only in this training lab**:

```text
1' OR '1'='1' -- -
```

The page returned five records instead of one:

| First name | Surname |
| --- | --- |
| admin | admin |
| Gordon | Brown |
| Hack | Me |
| Pablo | Picasso |
| Bob | Smith |

The always-true condition changes the query's filtering logic, and the trailing SQL comment prevents the application's remaining quote from breaking the statement. This is direct evidence of SQL injection in the intentionally vulnerable DVWA configuration.

## Demonstrated impact and limits

**Confidentiality:** The test disclosed additional user names beyond the single record requested by ID. In a real application, an equivalent weakness could expose data outside the intended lookup, depending on database permissions and accessible data.

**Not demonstrated:** Password disclosure, data modification, data deletion, privilege escalation, or effects on a real organization. No CVSS score or production severity is assigned from this training exercise alone.

## Recommended remediation

1. Use **parameterized queries / prepared statements**, binding the User ID as a value rather than concatenating it into SQL text.
2. Apply least-privilege database permissions and enforce application-level access controls so a user can access only authorized records.
3. Return generic error messages to users and log detailed database errors securely. **Hiding errors alone does not fix SQL injection.**
4. Validate the expected format of the User ID as an additional control, not as a replacement for parameterization.

## Retest plan — pending

After implementing a fix, verify that a normal lookup for ID `1` still returns only the intended record, that malformed input is safely handled, and that the previously successful crafted input no longer changes the query or returns additional records. Check the actual page security level and cookie state before comparing results.

## Evidence status

The observations above were recorded during the interactive lab and supported by screenshots shown during the session. **Screenshot files have not been added to this repository**, and remediation/retesting have not been performed. Add appropriately cropped screenshots showing the Low security setting, baseline result, syntax error, and five-record result if evidence images are later committed. Do not publish session cookies or other sensitive tokens.

## Learning takeaway

Untrusted input must remain **data**, not become SQL **instructions**. A valid baseline, careful control tests, and verification of the application configuration are essential to interpreting security-test results.

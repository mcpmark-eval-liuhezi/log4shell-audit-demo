# Advisory Summary: Log4Shell

## GHSA-jfh8-c2jp-5v3q — Remote code injection in Log4j

| Field | Value |
| --- | --- |
| GHSA ID | GHSA-jfh8-c2jp-5v3q |
| CVE | CVE-2021-44228 |
| Severity | Critical (CVSS 3.1 score 10.0, vector `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H/E:H`) |
| Affected package | `org.apache.logging.log4j:log4j-core` (Maven) |
| Vulnerable versions | All versions prior to 2.15.0 (<= 2.14.1, plus the EOL 1.x line) |
| First patched version | 2.15.0 (2.16.0 recommended — disables JNDI lookups by default and removes message-lookup support; backports available in 2.3.1, 2.12.2, 2.12.3) |
| Advisory link | https://github.com/advisories/GHSA-jfh8-c2jp-5v3q |

## Summary

Log4j versions prior to 2.16.0 are subject to a remote code execution vulnerability via the
LDAP JNDI parser. Apache Log4j2 <= 2.14.1 JNDI features used in configuration, log messages,
and parameters do not protect against attacker-controlled LDAP and other JNDI-related
endpoints. An attacker who can control log messages or log message parameters can execute
arbitrary code loaded from LDAP servers when message lookup substitution is enabled.

## Impact

Logging untrusted or user-controlled data with a vulnerable version of Log4j may result in
remote code execution (RCE) against your application. This includes untrusted data included
in logged errors such as exception traces, authentication failures, and other unexpected
vectors of user-controlled input.

## Affected packages

Only the `org.apache.logging.log4j:log4j-core` package is directly affected by this
vulnerability. The `org.apache.logging.log4j:log4j-api` should be kept at the same version as
`org.apache.logging.log4j:log4j-core` to ensure compatibility if in use.

## References

- GitHub Security Advisory: https://github.com/advisories/GHSA-jfh8-c2jp-5v3q
- NVD: https://nvd.nist.gov/vuln/detail/CVE-2021-44228
- Apache Log4j security guide: https://logging.apache.org/log4j/2.x/security.html
- Follow-up advisory (CVE-2021-45046): https://github.com/advisories/GHSA-7rjr-3q55-vv33

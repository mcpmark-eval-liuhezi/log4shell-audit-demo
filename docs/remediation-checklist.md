# Remediation Checklist: Log4Shell (CVE-2021-44228)

Reference: `docs/advisory-log4shell.md` — GHSA-jfh8-c2jp-5v3q

## Upgrade

- [ ] **Upgrade past the patched version.** Bump `org.apache.logging.log4j:log4j-core`
      past the first patched version (2.15.0). Target 2.16.0 or later, which disables JNDI
      lookups by default and removes message-lookup support. If pinned to an older line,
      use the available backports: 2.3.1, 2.12.2, or 2.12.3.
- [ ] Keep `org.apache.logging.log4j:log4j-api` at the same version as `log4j-core`.
- [ ] Confirm no override/resolution pins an older `log4j-core` in the dependency tree.

## Scan

- [ ] **Scan the dependency tree for other affected artifacts.** Run a full transitive
      dependency audit (e.g. `mvn dependency:tree`, `gradle dependencies`, or a
      SCA tool such as Dependabot/OWASP Dependency-Check/Trivy) and list every artifact
      that pulls in `log4j-core` < 2.15.0.
- [ ] Record each affected artifact, its version, and the owning team for the roll-up report.

## Verify

- [ ] **Verify the upgrade in CI.** Add/extend a CI step that fails the build if any
      resolved `log4j-core` version is below 2.16.0.
- [ ] Confirm application logging still works and no JNDI lookups are triggered by
      log message lookup substitution.
- [ ] Confirm test suite and integration tests pass on the upgraded dependency set.

## Sign off

- [ ] **Sign off.** Record the remediated version, the audit trail link, and the date.
- [ ] Close the tracking issue only after CI is green and the sign-off entry is recorded.

## Notes

- Log4j 1.x is End of Life and will not receive patches for this issue; it is also
  vulnerable to other RCE vectors. Migrate to 2.16.0 where possible.
- 2.15.0's fix did not disable attacker-controlled JNDI lookups in all situations —
  see CVE-2021-45046 / GHSA-7rjr-3q55-vv33 — which is why 2.16.0+ is the recommended target.

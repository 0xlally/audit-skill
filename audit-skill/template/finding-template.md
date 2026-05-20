# Audited Vulnerability Submission Template

Use only for `Submit-ready` findings or submission drafts requested by the user. Keep the report conservative, reproducible, and maintainer-facing.

Chinese version: `finding-template.zh.md`.

```markdown
# Audited Vulnerability Submission: <Short title>

## Audit Verdict
- Verdict: Submit-ready | Revise before submission | Needs validation | Downgrade | Reject
- Audited severity: Critical | High | Medium | Low | Informational | None
- Evidence level: E0 | E1 | E2 | E3 | E4
- Confidence: Confirmed | Likely | Possible | Rejected
- One-line reason: <why this verdict is correct>

## Scope
- Affected project: <project name>
- Affected version/commit: <version, tag, commit hash>
- Affected component: <component/path/package>
- Tested environment: <OS, runtime, dependency versions, config, deployment mode>
- Default/common configuration affected: Yes | No | Unknown
- Submission channel: SECURITY.md | Private advisory | Security email | Bug bounty platform | CNA | Other

## Claim Delta
- Original claimed severity/CVSS: <original claim>
- Audited severity/CVSS: <audited vector and score>
- Changed from original claim? No | Yes: <downgrade/upgrade/wording change and why>
- Suggested CWE: <CWE ID if known>

## Summary
<Explain the vulnerability in maintainer language. State the security boundary crossed and avoid speculative impact.>

## Attacker Model
- Required privilege: None | Low-privilege user | Authenticated user | Admin | Other
- Network position: Remote | Same network | Local | Other
- User interaction: Not required | Required
- Attack complexity: Low | High
- Required configuration: Default | Common | Non-default
- Attacker-controlled source: <exact parameter/body/header/file/message/state and how attacker controls it>
- Explicit prerequisites: <all required conditions, including any chained bug or social engineering>

## Reachability and Root Cause
- Entry point: `<route/command/API/parser/job>`
- Vulnerable code path: `<file/function>`
- Sink or security decision: `<sink/check>`
- Barriers reviewed: <auth, authorization, tenant checks, sanitizers, allowlists, escaping, normalization, type checks>
- Root cause: <why the barrier is missing, bypassed, or insufficient>

## Reproduction
1. <Deploy the affected version in a local or authorized test environment.>
2. <Apply only the required configuration.>
3. <Authenticate as the required role, or state no auth is needed.>
4. <Send the minimal non-destructive request/input.>
5. <Observe the concrete security impact.>

## Evidence
- PoC summary: <minimal, safe, non-destructive>
- Request/response or CLI transcript: <redacted summary or path>
- Logs/traces/screenshots: <path or summary>
- Causal proof: <why the observed result is caused by the vulnerability>
- Negative controls: <blocked role/config/input, patched version, or barrier test if available>

## Impact Proof Matrix
- Confidentiality: None | Low | High; <exact data read and why sensitive>
- Integrity: None | Low | High; <exact state changed and why unauthorized/high value>
- Availability: None | Low | High; <exact outage/crash/deletion/resource exhaustion>
- Scope changed: Unchanged | Changed; <specific security boundary crossed>
- Practical impact: <RCE, auth bypass, privilege escalation, file access, SSRF, SQLi data access, tenant break, account takeover, etc.>

## Graph Evidence
- CodeQL/data-flow evidence used: Yes | No
- Database/query: <language, commit, coverage notes, query path, or N/A>
- Source-to-sink path: <summary or N/A>
- Manual graph review result: <why the path is real or why graph evidence is not relied on>
- False-positive risks handled: <barriers, unreachable paths, broad taint steps, missing coverage>

## Severity Rationale
- CVSS vector and score: <CVSS v3.1 or v4.0 vector>
- Vector justification: <AV/AC/PR/UI/S/C/I/A or v4 equivalents from verified facts>
- High threshold met: Yes | No | N/A
- Critical threshold met: Yes | No | N/A
- Inflation checks: <why severity is not overstated; note any downgrade factors>

## Maintainer Rejection Simulator
- Strongest likely rejection: <documented behavior, intended design, duplicate, fixed, admin-only, non-default config, low impact, out of scope, etc.>
- Evidence against rejection: <facts that refute it>
- Prior-art search: <docs/advisories/CVE/changelog/issues/commits checked and result>
- Remaining uncertainty: <unknowns maintainers may ask about>
- Submission recommendation: Submit | Revise first | Do not submit

## Remediation Guidance
<Describe the smallest safe fix direction, relevant tests, and compatibility concerns. Do not include a full patch unless requested.>

## Disclosure Notes
- Sensitive details redacted: Yes | No | N/A
- Destructive payload omitted: Yes | No | N/A
- Suggested initial message: <short maintainer-facing summary>
- Coordinated disclosure notes: <embargo/timeline/policy notes>
```

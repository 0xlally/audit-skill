# Audit Skill

This is an agent-oriented submission-readiness skill. It is not a vulnerability-hunting guide. Use it after an agent or researcher has produced a vulnerability candidate and you need a strict reviewer to decide whether the finding is submit-ready, overstated, under-evidenced, duplicated, intended behavior, or a false positive.

Chinese version: [README.zh.md](README.zh.md).

## Purpose

The skill is designed to reduce the review burden caused by agent-generated vulnerability reports:

- Static code findings presented as real vulnerabilities.
- Theoretical attack chains presented as proven exploitability.
- Dangerous sinks presented as High/Critical impact without real impact proof.
- CVSS vectors adjusted to meet a target severity.
- Missing checks for documentation, prior fixes, advisories, issue history, supported versions, and bounty scope.

The intended outcome is simple: if a finding passes this skill, the remaining report should be conservative, reproducible, maintainer-facing, and ready to submit.

## Workflow

The skill reviews existing candidates through eight gates:

1. Freeze the original claim and separate facts from assumptions.
2. Assign evidence level `E0` to `E4`.
3. Validate reachability and security barriers.
4. Validate attacker control and prerequisites.
5. Prove actual confidentiality, integrity, availability, and scope impact.
6. Run the maintainer rejection simulator.
7. Recalculate CVSS and remove inflated wording.
8. Produce a submission package only when the finding is defensible.

## Verdicts

- `Submit-ready`: evidence is sufficient and the report can be sent.
- `Revise before submission`: real or plausible, but the report needs fixes.
- `Needs validation`: key runtime, version, or impact proof is missing.
- `Downgrade`: real issue, inflated severity or impact.
- `Reject`: false positive, duplicate, intended behavior, out of scope, or no security impact.

## Install

Install it in Codex with `$skill-installer` from:

```text
https://github.com/0xlally/audit-skill
```

You can also install from repository `0xlally/audit-skill`.

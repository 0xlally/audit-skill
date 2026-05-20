---
name: audit-skill
description: "Submission-readiness vulnerability audit workflow for agent-generated or researcher-supplied findings. Use when Codex needs to review an existing vulnerability candidate, report, PoC, CodeQL result, or CVE draft before disclosure; detect false positives, duplicated or intended behavior, weak evidence, unreachable paths, attacker-model mistakes, inflated CVSS/severity, overstated impact, maintainer rejection risk, and produce a maintainer-ready final report. This skill does not guide vulnerability hunting."
---

# Audit Skill

Chinese version: `SKILL.zh.md`.

## Goal

Audit existing vulnerability candidates so that only findings likely to survive maintainer, bounty triager, CNA, or CVE review are submitted.

Do not hunt for new vulnerabilities. If the user provides only a repository and no candidate, ask for the candidate/report/PoC to review. Review nearby code only to validate the submitted claim.

Treat every candidate as unconfirmed until it passes evidence, reachability, attack-model, impact, duplicate/prior-art, and severity gates. The default bias is not pessimism; it is submission quality.

Final verdicts:

- `Submit-ready`: evidence is sufficient, severity is defensible, rejection risk is handled, and the report can be sent.
- `Revise before submission`: the issue is plausible or real, but wording, evidence, reproduction, scope, or CVSS must be fixed first.
- `Needs validation`: the claim may be valid, but essential runtime or version evidence is missing.
- `Downgrade`: a real issue exists, but the claimed severity or impact is inflated.
- `Reject`: false positive, duplicate, intended/documented behavior, out of scope, or not security-impacting.

## Non-Negotiable Rules

- Do not invent missing evidence or convert assumptions into facts.
- Do not label High without evidence level `E3`; do not label Critical without evidence level `E4`.
- Do not mark `Submit-ready` while any essential item is unknown: affected version, default/common configuration, attacker-controlled source, reachable path, demonstrated impact, duplicate/fix status, or CVSS vector.
- Do not raise severity to meet a target. Recalculate severity from the verified attack model and concrete impact.
- Do not modify the target project source code unless the user explicitly asks for remediation.

## Audit Gate Workflow

Run the gates in order. Stop early when a blocking rejection is proven; otherwise keep a concise audit trail.

1. Intake and claim freeze.
   - Capture the exact submitted claim before judging it: title, affected project/version/commit, component, entry point, source, sink, trust boundary, prerequisites, claimed impact, PoC, evidence, claimed CVSS/CWE, and intended submission channel.
   - Separate facts from author assumptions. Quote or summarize the original severity claim so later changes are explicit.
   - If the candidate is underspecified, list the missing fields and continue only as `Needs validation`.

2. Evidence floor.
   - Assign an evidence level from `E0` to `E4`.
   - Static code review, grep hits, dangerous functions, or CodeQL alerts alone are not enough for submission.
   - Verify the test environment: exact commit/tag, dependency versions, configuration, feature flags, platform, auth state, and whether the setup matches default or common deployment.
   - Record whether the PoC is minimal, non-destructive, reproducible, and causally tied to the claimed impact.

3. Reachability and barrier check.
   - Confirm the entry point is real, enabled, reachable under the claimed network position, and not limited to tests, debug routes, local-only tools, admin-only panels, or internal harnesses unless those are explicitly claimed as prerequisites.
   - Trace source to sink with the relevant auth checks, permission checks, tenant checks, parser behavior, sanitizers, allowlists, escaping, normalization, type checks, and other barriers.
   - If a barrier exists, decide whether it blocks exploitation, narrows scope, changes prerequisites, or only partially mitigates impact.
   - For graph/CodeQL evidence, manually review the path. A query hit is evidence of a path to inspect, not evidence of a vulnerability.

4. Attacker model validation.
   - Prove how the attacker controls the source. Do not assume control of privileged browser contexts, local services, admin pages, test harnesses, third-party origins, or deployment internals.
   - Verify `AV`, `AC`, `PR`, and `UI` from the demonstrated path, not from the desired score.
   - If control depends on another bug, malicious CDN, supply-chain compromise, browser extension, social engineering, user-pasted code, non-default config, or trusted administrator action, write that prerequisite explicitly and adjust severity.

5. Impact proof matrix.
   - Require concrete evidence for each claimed high-impact dimension.
   - `C:H`: what non-public sensitive data was actually read, from whose account/tenant, and can it be reused?
   - `I:H`: what high-value state was actually changed, was it unauthorized, persistent, and across a security boundary?
   - `A:H`: what service outage, deletion, crash, resource exhaustion, or core-function loss was actually caused?
   - `S:C`: which security boundary was crossed, from which security authority to which other authority?
   - Function names, schemas, controllers, registries, sinks, or "could lead to" language do not prove High impact by themselves.

6. Maintainer rejection simulator.
   - Write the strongest likely maintainer rejection, then try to refute it with evidence.
   - Reject or downgrade if the behavior is documented, intended, already disclosed, fixed in supported releases, dependent on a trusted administrator, dependent on user-chosen insecure deployment, duplicate of a known issue, limited to unsupported/EOL versions, inside test/demo code only, caused by third-party misuse, or too low impact.
   - Search official docs, security policy, advisories, GitHub Security Advisories, NVD/CVE, changelogs, release notes, commits, issues, and bounty scope when network access is available.
   - If the rejection cannot be refuted, do not submit as claimed.

7. Severity and wording audit.
   - Recalculate CVSS v3.1 or v4.0 from verified facts. Use the lowest defensible vector when multiple interpretations are possible.
   - High requires suggested CVSS >= 7.0 and `E3`; Critical requires suggested CVSS >= 9.0 and `E4`.
   - CVSS thresholds are necessary but not sufficient. The practical impact must match the label.
   - Rewrite speculative language into measured claims: "allows" only when proven, "may allow" only for unconfirmed paths, and no system-wide language unless system-wide impact was demonstrated.

8. Submission package.
   - For `Submit-ready`, use `template/finding-template.md` to produce a maintainer-facing report with exact versions, prerequisites, reproduction, evidence, impact, CVSS/CWE, fix guidance, duplicate checks, and disclosure channel.
   - For non-submit verdicts, output blocking gaps and the minimum evidence or wording changes needed to make the candidate reviewable.
   - Keep payloads safe and redact secrets, production tokens, private user data, and destructive details.

## Evidence Levels

Every candidate must have one evidence level:

- `E0`: keyword/rule match, dangerous API, suspicious pattern, or unreviewed alert only. Never submit.
- `E1`: static source-to-sink path appears plausible, but entry point, config, privileges, or runtime behavior is unverified. At most `Possible`.
- `E2`: local/test validation reaches the sink, but default configuration, prerequisites, or security impact is incomplete. Usually `Needs validation` or `Downgrade`.
- `E3`: default or common configuration demonstrates a full non-destructive attack chain with clear security impact. Required for `Confirmed High`.
- `E4`: default or common configuration demonstrates low-complexity system-level, cross-tenant, control-plane, key/credential, account-takeover, or large-scale data compromise. Required for `Confirmed Critical`.

No `E3`, no `Confirmed High`. No `E4`, no `Confirmed Critical`.

## Output Rules

Lead with the verdict and one-sentence reason.

Use these groups when there are multiple candidates: `Submit-ready`, `Revise before submission`, `Needs validation`, `Downgraded`, and `Rejected`.

For each candidate, include a compact card:

- Title and verdict
- Claimed severity vs audited severity
- Evidence level
- Affected version/component
- Required attacker model and prerequisites
- Verified impact
- Key proof or missing proof
- Maintainer rejection risk
- Required next action

Only create a full report with `template/finding-template.md` when the finding is `Submit-ready` or the user asks for a submission draft.

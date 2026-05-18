# CVE Skill

This is an agent-oriented skill. I mainly use it with GPT-5.4. It supports authorized high-impact vulnerability research in open-source projects, with a focus on credible High/Critical candidates and responsible disclosure evidence. It is recommended to use it together with Codex goal mode.

Chinese version: [README.zh.md](README.zh.md).

## Outputs

| CVE | CVSS | Severity |
| --- | --- | --- |
| CVE-2026-45727 | 8.2 | High |

Users of this skill are welcome to submit their own outputs through issues or pull requests. Please submit only authorized, already public, or non-sensitive information.

## Install

Install it in Codex with `$skill-installer` from:

```text
https://github.com/0xlally/CVE-skill/tree/main/cve-skill
```

You can also install from repository `0xlally/CVE-skill` and path `cve-skill`.

## Regular Mode

There are two observations from using Codex for vulnerability research:

- First, the false-positive rate can be high, including inflated severity ratings and failing to check official security notes. This creates a lot of review work. Prompting can reduce the problem, but repeating the same prompt every time is tedious, so this skill exists to make those guardrails reusable.
- Second, Codex tends to focus heavily on authentication, authorization bypass, configuration, and parameters. It is weaker at covering other bug classes. This skill guides the audit toward high-impact vulnerabilities.

## Graph Mode

Graph mode extends regular mode with a different hunting strategy. If regular mode has run for long enough and still finds no vulnerabilities, code auditing starts to look like human reasoning: inspect possible security issues in the project, trace fixes, and try to bypass them. But the code Codex sees is incomplete. Auditing every function can reduce omissions, but many functions are not exploitable by themselves. A vulnerability usually needs a complete chain from entry point to impact, so function-by-function auditing is inefficient. Graph mode addresses this by:

- Using CodeQL to build function and data-flow relationship graphs, expanding Codex's view and covering more paths.
- Recording audited nodes to avoid ineffective repetition.

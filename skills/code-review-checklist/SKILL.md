---
name: code-review-checklist
description: Reviews staged or branch-level code changes against a comprehensive checklist covering security, performance, readability, correctness, and tests. Use this before requesting a review or merging a PR.
category: code-quality
usage_examples:
  - "Review my changes"
  - "Run the code review checklist on this PR"
  - "Check my staged changes against the code review checklist"
  - "Do a code review of this branch"
---

You are performing a structured code review against the checklist defined in `references/checklist.md`. Follow each step in order.

## Step 1 — Collect the Changes

Run `git diff main...HEAD` (or `git diff --staged` if reviewing staged-only changes) to get the full diff. Also run `git diff main...HEAD --stat` to understand the scope.

If the user specifies a particular file or directory, focus the review there but note if related files outside that scope appear relevant.

## Step 2 — Load the Checklist

Read `references/checklist.md` from this skill's directory. Each section contains review criteria organized by category. You will evaluate every applicable item against the diff.

## Step 3 — Review Against Each Category

Work through every checklist category. For each item:
- Mark as **Pass**, **Fail**, **Warning**, or **N/A**
- For any Fail or Warning, quote the relevant code snippet (file path + line reference) and explain the specific concern
- Do not mark items as N/A without brief justification

Prioritize findings:
- **Critical**: security vulnerabilities, data loss risk, crashes in production paths
- **High**: correctness bugs, broken error handling, missing auth checks
- **Medium**: performance issues, missing tests for changed logic, confusing code
- **Low**: style, naming, minor readability

## Step 4 — Identify Positive Patterns

Note 1–3 things done particularly well. Code review should be constructive, not only critical.

## Step 5 — Produce the Review Report

Output a structured Markdown report. Be specific and actionable — every issue must include a file reference and a suggested fix or improvement direction.

---

## Code Review Report

**Branch diff against:** main  
**Files changed:** (count) | **Insertions:** (+N) | **Deletions:** (-N)

### Summary

1–3 sentence overall assessment. State whether you recommend: Approve / Approve with minor comments / Request changes.

### Critical Issues
(If none: "None found.")

Each entry format:
> **[CRITICAL]** `path/to/file.ts:42` — Description of the issue.  
> **Why it matters:** ...  
> **Suggested fix:** ...

### High Issues
(Same format.)

### Medium Issues
(Same format.)

### Low Issues / Style Notes
(Bullet list, file references, brief descriptions.)

### Checklist Summary

| Category | Status | Notes |
|---|---|---|
| Security | Pass / Fail / Warning | |
| Error Handling | | |
| Performance | | |
| Correctness | | |
| Tests | | |
| Readability | | |
| Types & Interfaces | | |
| Dependencies | | |
| Logging & Observability | | |
| Documentation | | |

### What's Done Well

- (1–3 positive observations)

### Recommended Next Steps

Numbered list of the most important actions before this code ships, in priority order.

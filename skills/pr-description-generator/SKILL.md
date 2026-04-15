---
name: pr-description-generator
description: Generates a comprehensive, well-structured PR description from the current branch's diff and commit history. Use this before opening a pull request to save time and ensure reviewers have the context they need.
category: code-quality
usage_examples:
  - "Generate a PR description for this branch"
  - "Write the PR description"
  - "Create a pull request description for my changes"
  - "Generate a PR description targeting main"
---

You are generating a comprehensive pull request description from the current branch's changes. Follow each step in order.

## Step 1 — Gather Branch Context

Run the following git commands to understand the scope of changes:
- `git log main..HEAD --oneline` — list all commits on this branch
- `git diff main...HEAD --stat` — summarize files changed, insertions, deletions
- `git diff main...HEAD` — full diff (read carefully; do not output it verbatim)

If the base branch is not `main`, infer it from context or ask the user.

## Step 2 — Classify the Change Type

Based on the diff, determine the primary change type(s):
- **feat** — new feature or capability
- **fix** — bug fix
- **refactor** — restructuring without behavior change
- **perf** — performance improvement
- **test** — adding or updating tests
- **docs** — documentation only
- **chore** — build, config, dependency updates
- **style** — formatting, linting (no logic changes)

A PR may span multiple types — note the dominant one.

## Step 3 — Identify Key Changes

Analyze the diff and extract:
- What was added, changed, or removed (group by feature area, not by file)
- Any breaking changes (API signature changes, removed exports, renamed routes, schema migrations)
- External dependencies added or removed (check package.json changes)
- Configuration or environment variable changes
- Database migrations or data model changes
- UI/UX changes visible to end users

## Step 4 — Assess Risk and Testing

Evaluate:
- Which areas of the codebase are touched (surface area of the change)
- Whether tests were added or updated
- Whether the change touches auth, payments, data persistence, or public APIs (flag as higher risk)
- Known limitations or follow-up work needed

## Step 5 — Write the PR Description

Produce the description in the following Markdown format. Keep each section concise and factual — avoid padding.

---

## Summary

Brief 1–3 sentence overview of what this PR does and why. State the problem being solved or the feature being added.

## Changes

- (Bullet list of concrete changes grouped by area, e.g. "Auth", "UI", "API", "Tests")
- Focus on what changed and why, not which files were touched
- Call out breaking changes with a **Breaking:** prefix

## Type of Change

- [ ] Bug fix
- [ ] New feature
- [ ] Refactor / code cleanup
- [ ] Performance improvement
- [ ] Tests
- [ ] Documentation
- [ ] Build / config / dependencies
- [ ] Breaking change

## How to Test

Step-by-step instructions a reviewer can follow to verify the change works correctly. Be specific: include routes to visit, actions to perform, expected outcomes.

1. (step)
2. (step)
3. Expected: (what should happen)

## Screenshots / Recordings

(Leave blank or note "N/A — no UI changes" if not applicable. If UI changed, add a placeholder: `<!-- Add before/after screenshots here -->`)

## Checklist

- [ ] Tests added or updated
- [ ] No console errors or warnings introduced
- [ ] No hardcoded secrets or debug code
- [ ] Documentation updated (if public API or config changed)
- [ ] Breaking changes documented above

---

Do not include file paths as a list of "changed files" — GitHub shows that automatically. Focus on meaning, not mechanics.

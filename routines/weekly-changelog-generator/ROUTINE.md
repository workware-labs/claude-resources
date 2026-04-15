---
name: Weekly Changelog Generator
description: Review all git commits from the past 7 days and generate a clean, user-facing changelog grouped by features, fixes, and internal improvements.
category: content
model: Sonnet 4.6
trigger_type: schedule
trigger_config:
  cron: "0 16 * * 5"
connectors: []
tools_used:
  - bash
  - read_file
---

You are generating the weekly user-facing changelog. Every Friday afternoon you review the week's commits and produce a polished changelog entry ready to be published or shared with customers.

## Steps

1. **Collect commits from the past 7 days.**
   Run: `git log --since="7 days ago" --oneline --no-merges --format="%H %s"` to get commit hashes and subjects.
   Then for each commit of interest, run `git show <hash> --stat` to understand which files changed.

2. **Filter out noise.** Skip commits that are purely:
   - Dependency version bumps with no user impact (e.g., "chore: bump eslint to 8.x")
   - CI/CD configuration changes
   - Internal tooling or developer-experience-only changes
   - Merge commits or revert-of-revert commits

3. **Classify each remaining commit** into one of these categories:
   - **New Feature** — new functionality visible to end users
   - **Improvement** — enhancement to existing functionality (performance, UX, accessibility)
   - **Bug Fix** — corrects a broken or incorrect behavior
   - **Security** — addresses a security vulnerability (always include, even if internal)
   - **Breaking Change** — changes existing behavior in a way that may require user action
   - **Internal / Chore** — refactors, test additions, dependency updates worth noting

4. **Write user-facing descriptions.** For each commit in the non-noise categories:
   - Rewrite the commit message in plain English, as if explaining to a non-technical user
   - Focus on the impact ("You can now…", "Fixed an issue where…", "Improved the speed of…")
   - Drop implementation details unless they are relevant to the user
   - If a commit message references a ticket number (e.g., `#123`), link it

5. **Group and sort.** Order sections: Breaking Changes first (if any), then New Features, Improvements, Bug Fixes, Security, Internal.

6. **Write a one-paragraph summary** at the top of the changelog entry that highlights the 2–3 most impactful changes in plain language. Keep it under 80 words.

## Output Format

## [Week of YYYY-MM-DD]

> [One-paragraph summary of the week's highlights.]

### Breaking Changes
- Description of breaking change. **Action required:** what the user must do.

### New Features
- Description of new feature. ([#ticket](link) if available)

### Improvements
- Description of improvement.

### Bug Fixes
- Description of fix.

### Security
- Description of security fix.

### Internal
- Description of internal change (brief, one line each, batched if many).

---
*[n] commits by [n] contributors this week.*

## Notes for the team
After generating, note any commits that were ambiguous to classify, or any areas where commit messages were too vague to produce a user-facing description. Suggest better commit message conventions if a pattern of vague messages is detected.

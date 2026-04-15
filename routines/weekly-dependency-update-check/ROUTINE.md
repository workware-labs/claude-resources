---
name: Weekly Dependency Update Check
description: Audit npm, pip, or cargo dependencies for outdated packages, known CVEs, and breaking-change version bumps. Produces a triage report and draft update plan.
category: devops
model: Opus 4.6 1M
trigger_type: schedule
trigger_config:
  cron: "0 10 * * 3"
connectors: []
tools_used:
  - bash
  - read_file
---

You are running the weekly dependency audit. Your goal is to surface outdated packages, known security vulnerabilities, and actionable upgrade paths so the team can maintain a healthy dependency graph without surprise breakages.

## Steps

### 1. Detect Package Manager(s)

Check the repo root for the following lockfiles and configuration files to determine which package managers are in use:
- `package.json` / `package-lock.json` / `yarn.lock` / `pnpm-lock.yaml` → Node.js (npm / yarn / pnpm)
- `requirements.txt` / `pyproject.toml` / `Pipfile.lock` → Python (pip / poetry / pipenv)
- `Cargo.toml` / `Cargo.lock` → Rust (cargo)

Run the appropriate outdated check for each detected manager:
- npm: `npm outdated --json`
- yarn: `yarn outdated --json`
- pnpm: `pnpm outdated --format json`
- pip: `pip list --outdated --format json`
- cargo: `cargo outdated -R --format json` (if cargo-outdated is installed)

### 2. Security Audit

Run the security audit for each manager:
- npm: `npm audit --json`
- yarn: `yarn audit --json`
- pnpm: `pnpm audit --json`
- pip: `pip-audit --format json` (if pip-audit is installed; otherwise check PyPI advisories manually for top 20 deps)
- cargo: `cargo audit --json`

Classify each CVE by severity: critical, high, moderate, low.

### 3. Categorize Updates

For each outdated package, classify the update type:
- **Patch** (e.g., 1.2.3 → 1.2.4): safe to apply, no API changes expected
- **Minor** (e.g., 1.2.x → 1.3.x): usually safe, check changelog for deprecations
- **Major** (e.g., 1.x.x → 2.x.x): breaking changes likely, requires manual review

Flag any package where a major version bump has been available for more than 90 days — these represent long-standing technical debt.

### 4. Review Changelogs for Major Bumps

For each package with a pending major version update, look up the changelog or migration guide (use the package's npm/PyPI/crates.io page or GitHub releases). Summarize breaking changes in 1–2 sentences.

### 5. Identify Deprecated Packages

Flag any package that has been officially deprecated (marked deprecated on npm/PyPI/crates.io) or has had no releases in the past 18 months. Suggest a maintained alternative where one exists.

## Output Format

### Summary
- Package manager(s) checked
- Total dependencies
- Outdated: [n] (patch: n, minor: n, major: n)
- Security vulnerabilities: [n] (critical: n, high: n, moderate: n, low: n)
- Deprecated or unmaintained packages: [n]

### Security Vulnerabilities (fix immediately)
Table: Package | Installed | CVE ID | Severity | Fix Version | Notes

### Major Version Updates Available
Table: Package | Current | Latest | Months Pending | Key Breaking Changes | Effort (S/M/L)

### Minor / Patch Updates (safe to batch)
Table: Package | Current | Latest | Update Type

### Deprecated / Unmaintained Packages
Table: Package | Last Release | Status | Suggested Replacement

### Recommended Update Plan
1. This sprint (security + critical patches): list packages
2. Next sprint (minor bumps + major bumps with low effort): list packages
3. Backlog (major bumps requiring migration): list packages with estimated effort

### Notes
Any specific compatibility warnings, peer dependency conflicts, or ecosystem context the team should know about before updating.

---
name: Documentation Freshness Review
description: Every two weeks, check documentation files for accuracy against the current codebase — flag outdated API references, missing parameters, stale code examples, and broken links.
category: content
model: Sonnet 4.6
trigger_type: schedule
trigger_config:
  cron: "0 9 * * 1"
repository: your-org/your-repo
connectors: []
tools_used:
  - bash
  - read_file
  - glob
---

You are running the biweekly documentation freshness review. Documentation drift is one of the most common sources of developer frustration — your job is to find where the docs no longer match the code.

Note: This routine is scheduled every Monday but should only perform a full review every other week. Check whether the last full review was within 7 days; if so, output "Skipping — last full review was [date]." and stop.

## Scope

Review all documentation under:
- `/docs/` — primary documentation directory
- `/README.md` and any `README.md` files in subdirectories
- `/CONTRIBUTING.md`, `/ARCHITECTURE.md`, or similar top-level docs
- Any `.md` files co-located with source code that describe API usage or configuration

Exclude:
- `CHANGELOG.md` (covered by the changelog routine)
- Auto-generated API reference files (marked with a generation header)
- Files that have not changed in more than 2 years and have no corresponding active source code

## Steps

1. **Enumerate all in-scope documentation files.** Use glob patterns to find `*.md` files in the relevant paths. Record each file's path and last modified date from git: `git log -1 --format="%ai" -- <file>`.

2. **Check API and function references.** For each doc file, extract any mentions of:
   - Function or method names (e.g., `createUser()`, `fetchData()`)
   - Configuration option names (e.g., `API_KEY`, `timeout`, `retries`)
   - HTTP endpoints (e.g., `POST /api/users`)
   - CLI commands (e.g., `npm run build`, `vercel deploy`)

   For each reference, verify it still exists in the codebase using file search or grep. Flag references that do not match any current code.

3. **Validate code examples.** For each fenced code block in documentation:
   - Check that imported modules or packages still exist in `package.json` / `pyproject.toml` / `Cargo.toml`
   - Check that any function signatures in examples match the current function signatures in source files
   - Flag examples using deprecated APIs or syntax

4. **Check for stale version references.** Flag any mention of a specific version number (e.g., "requires Node 16", "compatible with v2.x") that conflicts with the current requirements in `package.json`, `.nvmrc`, `.node-version`, or similar.

5. **Verify internal doc links.** For each `[text](./relative/path)` link in documentation, confirm the target file or anchor exists. Flag broken links.

6. **Identify missing documentation.** Scan for exported functions, classes, or API routes in the codebase that have no corresponding documentation entry. Specifically look for:
   - Public functions in `/lib/` or `/utils/` with no JSDoc and no entry in docs
   - New API routes added in the past 30 days with no documentation entry
   - New CLI flags or configuration options with no docs update

## Output Format

### Review Date: [YYYY-MM-DD]
### Files Reviewed: [n]
### Issues Found: [n]

### Outdated API / Function References
Table: Doc File | Reference | Issue | Suggested Fix

### Stale Code Examples
Table: Doc File | Line | Example Summary | Issue (e.g., function renamed, package removed)

### Stale Version References
Table: Doc File | Reference | Current Requirement | Fix

### Broken Internal Links
Table: Doc File | Broken Link | Target That Is Missing

### Missing Documentation
Table: Source File | Symbol / Route | Date Added | Suggested Doc Location

### Files Not Updated in 6+ Months
Table: Doc File | Last Modified | Corresponding Source Files (if any) | Recommendation

### Recommended Actions
1. Prioritized list of the top 5 documentation updates to make this sprint, with estimated effort (S / M / L).
2. Note any systematic issues (e.g., "API docs consistently lag behind code by 2+ sprints — consider adding a doc update step to the PR template").

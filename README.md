# Claude Code Resources

A curated library of **routine recipes** and **skills** for [Claude Code](https://claude.ai/code). Browse the full collection with search and filtering at [workware.dev/routines](https://workware.dev/routines) and [workware.dev/skills](https://workware.dev/skills).

## What's in this repo?

### Routines

Routines are **scheduled Claude Code agents** that run automatically on a cron schedule, GitHub event, or API trigger. Each routine includes a prompt, trigger configuration, and MCP connector details.

| Category | Routines |
|----------|----------|
| SEO & Marketing | Weekly SEO Audit, Post-Deploy SEO Check, Weekly GSC Digest, Content Freshness Review |
| DevOps & CI/CD | Post-Deploy Smoke Test, Weekly Dependency Update Check, Daily Error Log Summary |
| Content & Writing | Weekly Changelog Generator, Documentation Freshness Review |
| Monitoring & Alerting | Daily Uptime & Performance Report, Weekly Lighthouse Audit |

### Skills

Skills are **on-demand capabilities** — markdown files you drop into your project's `.claude/skills/` directory. Claude automatically discovers and uses them during coding sessions.

| Category | Skills |
|----------|--------|
| SEO & Marketing | Blog Post from Keyword Cluster, SEO Page Optimizer |
| Code Quality | PR Description Generator, Code Review Checklist, Test Generator |
| Content & Writing | Technical Documentation Writer, API Reference Generator |

## Directory Structure

```
routines/
  weekly-seo-audit/
    ROUTINE.md          # YAML frontmatter + prompt
  ...

skills/
  seo-page-optimizer/
    SKILL.md            # YAML frontmatter + instructions
    scripts/            # (optional) executable code
    references/         # (optional) docs loaded into context
    assets/             # (optional) templates, icons, fonts
  ...
```

## Installing a Skill

**Download the .md file:**
```bash
curl -o .claude/skills/seo-page-optimizer.md https://workware.dev/api/skills/seo-page-optimizer/download
```

**Or clone just the skill you need:**
```bash
git clone --filter=blob:none --sparse https://github.com/alnoorp/claude-resources.git
cd claude-resources
git sparse-checkout set skills/seo-page-optimizer
```

**Or upload via Claude's web interface:**
Download the `.md` file and use the "Upload a skill file" option in Claude Code's web or desktop app.

## Using a Routine

1. Browse routines at [workware.dev/routines](https://workware.dev/routines)
2. Copy the prompt and trigger configuration
3. Create a new routine in Claude Code (CLI, web, or desktop)
4. Add the listed MCP connectors
5. Customize for your project

## File Format

### ROUTINE.md

```yaml
---
name: Weekly SEO Audit
description: Run a comprehensive SEO audit every Monday
category: SEO & Marketing
model: Opus 4.6 1M
trigger_type: schedule
trigger_config:
  cron: '0 9 * * 1'
connectors:
  - name: Rampify
    url: https://rampify.dev
tools_used:
  - crawl_site
  - get_issues
---

[Prompt text that Claude follows during each session]
```

### SKILL.md

```yaml
---
name: seo-page-optimizer
description: Analyze and optimize any page for SEO
category: SEO & Marketing
connectors:
  - name: Rampify
    url: https://rampify.dev
---

[Instructions that Claude follows when the skill is invoked]
```

## MCP Servers Referenced

- **[Rampify](https://rampify.dev)** — SEO analysis, keyword tracking, content optimization

## Contributing

This is currently a curated collection maintained by [Workware Labs](https://workware.dev). Community contributions via pull requests will be supported in a future version.

## License

MIT

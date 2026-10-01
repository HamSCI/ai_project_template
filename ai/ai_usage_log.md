# AI Usage Log: {{PROJECT_NAME}}

This log records every substantive AI-assisted session on the project "{{PROJECT_TITLE}}".

Required by the HamSCI Generative AI Use Agreement, and by any institutional or funder policy
that applies to this project (see `.claude/rules/ai-governance.md`).

**This log is the source of truth for what AI did on this project.** A disclosure paragraph in a
manuscript, poster, or software release summarizes this log truthfully. Keep it current: an
entry written from memory weeks later is not a record.

## Entry format

```
## [YYYY-MM-DD HH:MM TZ]
- **Tool**: Claude (Anthropic), <exact-model-id>
- **Session Purpose**: What the session set out to accomplish
- **Sections/Files Affected**: Specific files, sections, or documents touched
- **Nature of Contribution**: Draft / Edit / Analysis / Code generation / Research / Scaffolding
- **Human Review Status**: Reviewed and verified / Partially reviewed / Pending review
- **Git Hash**: <filled in after committing>
```

The date and time come from the system clock via `date`, never from an estimate. The `/commit`
command produces this format and appends it in the right order.

---

<!-- Append new entries below this line, newest at the bottom.
     On instantiating a new project from this template, delete every entry below:
     they belong to the template's own development, not to your project. -->

## [2026-09-22 20:43 UTC]
- **Tool**: Claude (Anthropic), claude-opus-5
- **Session Purpose**: Create the HamSCI `ai_project_template` by adapting
  `w2naf-academia/ai_project_template` for general HamSCI use: restructure the AI policy stack
  into three tiers so volunteer and student projects are not forced to carry institutional and
  funder sections, add HamSCI-specific rules for callsign attribution and station-location
  privacy, and add a getting-started onramp, LICENSE, CITATION.cff, and issue templates matching
  HamSCI house style.
- **Sections/Files Affected**: `README.md`, `CLAUDE.md`, `LICENSE`, `CITATION.cff`, `.gitignore`,
  `.claude/rules/ai-governance.md`, `.claude/rules/hamsci-data.md` (new),
  `.claude/rules/latex-writing.md`, `.claude/rules/python-code.md`, `.claude/commands/commit.md`,
  `.claude/settings.json`, `ai/ai_usage_log.md`, `docs/GETTING_STARTED.md`,
  `.github/ISSUE_TEMPLATE/` (bug_report, feature_request, question)
- **Nature of Contribution**: Scaffolding, drafting, and adaptation of an existing template
- **Human Review Status**: Pending review by N. A. Frissell (W2NAF)
- **Git Hash**: d1983ff

## [2026-10-01 16:26 UTC]
- **Tool**: Claude (Anthropic), claude-opus-5-5
- **Session Purpose**: Update the template so that every commit goes on a feature branch and
  through a pull request that a human reviews and merges, replacing the earlier
  "commit to main, ask before pushing" workflow.
- **Sections/Files Affected**: `.claude/commands/commit.md` (steps 6 to 9 rewritten for branch, PR, pointer bump, push order), `CLAUDE.md` (Commits and push paragraphs; Submodules order), `.claude/rules/python-code.md`, `.claude/rules/ai-governance.md`, `docs/GETTING_STARTED.md`, `README.md`
- **Nature of Contribution**: Edit
- **Human Review Status**: Pending review
- **Git Hash**: d96b578 (branch pr-workflow, PR HamSCI/ai_project_template#1), merged as 4276dd1

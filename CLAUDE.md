# {{PROJECT_NAME}}

## Project Overview
{{ONE-PARAGRAPH PROJECT DESCRIPTION: what is being written or built, its purpose, and its
audience.}}

**Lead**: {{LEAD_NAME_CALLSIGN_AND_AFFILIATION}}
**Collaborators**: {{COLLABORATORS: names, callsigns, and roles}}
**HamSCI Working Group**: {{WORKING_GROUP, or "none"}}
**Institution**: {{INSTITUTION, or "none: this is a volunteer project"}}
**Funder**: {{FUNDER_AND_GRANT_NUMBER, or "unfunded"}}
**Project period**: {{PROJECT_PERIOD}}

## Project Goal
{{PROJECT_GOAL, 1 to 3 sentences.}}

## Standing Rules

These two are binding on every HamSCI project and are imported here so they load into context
automatically:

@.claude/rules/ai-governance.md
@.claude/rules/hamsci-data.md

Two further rule files are optional, and are scoped by their own `paths:` frontmatter to the
file types they govern:

- `.claude/rules/latex-writing.md` applies to `.tex`, `.bib`, `.cls`, and `.sty` files
- `.claude/rules/python-code.md` applies to `.py`, `pyproject.toml`, and `requirements*.txt`

Delete whichever the project does not use. To have one of them load unconditionally instead, add
an `@` import line for it above.

## Repository Structure

This project starts from the HamSCI `ai_project_template` scaffold. Add or remove top-level
directories to match the project. The scaffold provides:

```
{{REPO_NAME}}/
├── CLAUDE.md                     ← this file; project instructions for Claude
├── README.md
├── LICENSE
├── CITATION.cff                  ← make the project citable
├── .gitignore
├── .gitmodules                   ← present only if you add submodules
├── .claude/
│   ├── settings.json
│   ├── commands/commit.md        ← /commit workflow
│   └── rules/
│       ├── ai-governance.md      ← required
│       ├── hamsci-data.md        ← required
│       ├── latex-writing.md      ← delete if no LaTeX
│       └── python-code.md        ← delete if no Python
├── .github/ISSUE_TEMPLATE/
├── ai/
│   └── ai_usage_log.md           ← mandatory AI session log
├── docs/
│   └── GETTING_STARTED.md        ← delete once the project is running
└── {{PROJECT-SPECIFIC FOLDERS}}  ← e.g. manuscript/, src/, notes/, posters/, data/
```

## Working Conventions

**Session notes.** Keep one dated notes file per working session in `notes/`, named
`YYYY-MM-DD_<topic>.md`, recording what was decided, why, what it depends on, and what is still
open. Write them for a reader with no context, which in practice means a future Claude session
and a future you.

**Commits.** Use the `/commit` command. It logs the AI session, commits submodules first, then
commits the main repo. Prefix AI-assisted commits with `[AI-assisted]`. Reference tracking
issues (`refs #N`, or `closes #N` only when completion is yours to declare).

**Never push without explicit instruction.** Never force-push or hard-reset. Fetch and verify
remote state before any push.

**Project boards and issue status are human-curated.** Read them freely; propose changes and
name the exact command rather than running it.

## Submodules (optional)

If the project includes submodules (an Overleaf manuscript, a separate code repository, a
hardware design repo), the commit and push order is fixed:

1. Commit **inside** the submodule
2. Commit the updated submodule pointer in this repo
3. Push the submodule
4. Push this repo

Never the reverse at either stage. A parent pushed ahead of its submodule looks correct on the
machine that did it and breaks for every clone, because the recorded pointer names a commit no
remote has. Verify before pushing a parent:

```bash
git -C <submodule> branch -r --contains HEAD   # empty output = local only; push the submodule first
```

Add submodules with:

```bash
git submodule add https://git.overleaf.com/<id> overleaf      # Overleaf manuscript
git submodule add git@github.com:HamSCI/<repo>.git <path>     # HamSCI code repo
```

The `/commit` workflow auto-detects submodules via `git submodule status`.

## AI Governance

Every substantive AI session is logged in `ai/ai_usage_log.md` **before** the work is committed.
Use `/commit`, which enforces the ordering. The full policy stack is in
`.claude/rules/ai-governance.md`, which is imported above.

# /commit: AI-Assisted Commit Workflow

Use this command any time you finish a substantive AI-assisted work session.

This workflow handles flat repos (no submodules), single-submodule repos (for example Overleaf only), and multi-submodule repos. Submodules are auto-detected from `git submodule status`.

## Steps

### 1. Get the current timestamp
```bash
date
```
Use this exact output. Never estimate the date/time.

### 2. Identify all changes
Check status across the main repo and every submodule:
```bash
git status
git submodule foreach 'git status'
```

Show diff summaries:
```bash
git diff --stat
git submodule foreach 'git diff --stat'
```

If `git submodule foreach` produces no output, the repo has no submodules; proceed without them.

### 2a. Check what is about to be staged
Before drafting the log entry, look at what changed and confirm that none of it is material that
must not be committed (see `.claude/rules/hamsci-data.md`):

- Operator addresses, phone numbers, or personal email addresses
- Fine-grained station coordinates tied to an individual
- Unpublished data from a collaborator or a project instrument
- Credentials, API keys, tokens, or `.env` files with real values
- LaTeX build artifacts, large binary data, or identifiable student records

If any callsigns or contributor names were added or changed in this session, verify them against
their source before committing. A misattributed observation is harder to retract than to prevent.

### 3. Ask the user for session purpose
Draft a purpose of this session for the AI log. Show it to the user, and ask them to confirm.

### 4. Draft the AI usage log entry
Use this format. Use the **actual running model ID** in the Tool field (e.g., `claude-opus-5`), not a placeholder.

```
## [YYYY-MM-DD HH:MM TZ]
- **Tool**: Claude (Anthropic), <actual-model-id>
- **Session Purpose**: [user's description]
- **Sections/Files Affected**: [list changed files and sections]
- **Nature of Contribution**: [Draft / Edit / Analysis / Code generation / Research / etc.]
- **Human Review Status**: [Reviewed and verified / Partially reviewed / Pending review]
- **Git Hash**: [fill in after committing]
```

Present the draft to the user. Wait for confirmation or corrections.

### 5. Append the entry to `ai/ai_usage_log.md`

### 6. Commit in submodules FIRST (if any have changes)
For each submodule with changes:
```bash
git -C <submodule-path> add <files>
git -C <submodule-path> commit -m "[AI-assisted] <description>"
```

If there are no submodules, or none have changes, skip this step.

### 7. Commit in the main repo
Stage `ai/ai_usage_log.md`, any updated submodule pointers, and any other changed project files:
```bash
git add ai/ai_usage_log.md <other-files-and-submodule-pointers>
git commit -m "[AI-assisted] <description>"
```

### 8. Fill in the git hash(es)
```bash
git log --oneline -1
```
Update the log entry's **Git Hash** field with the main-repo hash and any submodule hashes (e.g., `main=abc1234, overleaf=def5678`).

If updating the hash field requires a follow-up commit, use a non-`[AI-assisted]` commit message such as `Update AI usage log with git hashes`.

### 9. Ask before pushing
Never push without explicit user instruction.

When the user does authorize a push and the repo has submodules, push in this order:
**submodule first, then the parent.** Verify the submodule commit has reached a remote before
pushing the parent:

```bash
git -C <submodule-path> branch -r --contains HEAD
```

Empty output means the commit exists only on this machine, so the parent must wait. A parent
pushed ahead of its submodule looks correct locally and breaks for every clone.

## Notes

- The `[AI-assisted]` prefix applies only to commits whose content was produced or substantially shaped with AI assistance. Pure human edits (the user manually fixes a typo or rewords a sentence) should NOT carry the prefix.
- Commit submodules with their own `[AI-assisted]` prefix when their content is AI-assisted, separately from the main-repo pointer-bump commit.
- Use `git add <specific-files>` rather than `git add -A` or `git add .`, to avoid accidentally staging untracked artifacts (build outputs, credentials, large binaries).
- Write the commit message for someone reading it in two years with no context: what changed and why, not a restatement of the diff.
- Reference the tracking issue (`refs #N`, or `org/repo#N` when the issue lives in another repo). Use `closes #N` only when completion is yours to declare; it fires the moment the commit reaches the default branch, which silently overrides anyone who was asked to verify the fix and close it themselves.
- More than one session may be working the same repository at once. Run `git fetch && git status -sb` before starting a pass that will touch many files, not only before pushing.

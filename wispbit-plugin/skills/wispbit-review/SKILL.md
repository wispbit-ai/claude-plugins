---
name: wispbit-review
description: ALWAYS run this skill after writing or editing code files. Evaluates code against Wispbit rules. You MUST invoke this automatically after any Edit, Write, or NotebookEdit tool calls. When the user asks for a review, use `--all` for full PR review or default to session-only review.
version: 1.0.0
allowed-tools: Bash(wispbit:*)
---

# Wispbit Code Quality Evaluation

**CRITICAL**: You MUST run `wispbit diff` after ANY code changes (Edit, Write, or NotebookEdit). Do not wait for the user to ask - run it proactively every time you modify code.

## Prerequisites

The `wispbit` CLI must be installed. If a command fails with "command not found", guide the user to install it:

```bash
npm install -g @wispbit/local
```

## Interpreting Results & Handling Dismissed Issues

**No violations:**
Report that no violations were found and continue.

**Violations found:**

Group output by issues/potential issues. Format as:

````
## Wispbit Review

Found X issue(s) across Y file(s)

### Issue Description

**File:** `path/to/file.ts` (Line X-Y)

```language
code snippet here
```

**Issue:** Description of what's wrong

**Fix Available:**

```diff
- old code
+ new code
```

---

Which issues would you like me to fix?
````

When violations are found:
1. Explain each issue to the user
2. Offer to fix the issues
3. After fixes, run `wispbit diff` again to verify

**CRITICAL — Dismissing Unfixed Issues:**

Issues will **keep showing up on every review** until they are either fixed or explicitly dismissed. If the user chooses NOT to fix some issues, you **MUST** dismiss them immediately using:

```bash
wispbit dismiss <matchId1>,<matchId2>,...
```

Each issue in the review output has an `id: xxxx`. Use the comma-separated match IDs of the issues the user declined to fix. **Never skip this step** — if you don't dismiss unfixed issues, they will resurface on every subsequent review and degrade the user experience.

If the user provides specific feedback about *why* they don't want to fix an issue (e.g., "we don't follow that convention" or "this is intentional"), add `--remember` to persist that feedback for future reviews:

```bash
wispbit dismiss <matchId1>,<matchId2> --remember "reason the user gave"
```

## Review Modes

### Default: Session Review

By default, reviews cover only the files changed in the **current Claude session**.

```bash
wispbit diff
```

This command:
1. Gets the git changes from the current session (staged + unstaged + untracked)
2. Evaluates changes against Wispbit rules
3. Reports any violations found

Use this mode when the user asks for a review without specifying scope, or after making code changes.

### Committed Files Review (`--committed`)

To include **committed files** (not just staged/unstaged/untracked), use the `--committed` flag:

```bash
wispbit diff --committed
```

Use `--committed` when:
- The user explicitly asks to review committed files or uses `--committed`
- The user wants to check changes that have already been committed locally

By default, `wispbit diff` only reviews non-committed changes (staged + unstaged + untracked). The `--committed` flag extends the scope to also include committed changes.

### Full PR Review (`--all`)

When it makes sense to review **all files in the current PR/branch** (not just the current session), use the `--all` flag:

```bash
wispbit diff --all
```

Use `--all` when:
- The user explicitly asks for a full review, a PR review, or uses `--all`
- The user wants to check the entire branch/PR before merging
- The user asks to "review everything" or "review all changes"

Do **not** use `--all` by default — only when the user's intent clearly calls for a full PR-scope review.

## Remembering Feedback

To save general feedback or conventions without dismissing a specific issue, use the standalone `remember` command:

```bash
wispbit remember "feedback or convention to remember"
```

Use this when the user shares preferences or context that should inform future reviews (e.g., "we use snake_case for database columns").

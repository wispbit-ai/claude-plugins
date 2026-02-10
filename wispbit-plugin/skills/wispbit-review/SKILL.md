---
name: wispbit-review
description: ALWAYS run this skill after writing or editing code files. Evaluates code against Wispbit rules. You MUST invoke this automatically after any Edit, Write, or NotebookEdit tool calls. When the user asks for a review, run it for the current session.
version: 1.0.1
allowed-tools: Bash(wispbit:*)
---

Run the wispbit review using session ${CLAUDE_SESSION_ID}:

wispbit diff --claude-session-id ${CLAUDE_SESSION_ID}

# Wispbit Code Quality Evaluation

**CRITICAL**: You MUST run `wispbit diff --claude-session-id ${CLAUDE_SESSION_ID}` after ANY code changes (Edit, Write, or NotebookEdit). Do not wait for the user to ask - run it proactively every time you modify code.

## Prerequisites

The `wispbit` CLI must be installed. If a command fails with "command not found", guide the user to install it:

```bash
npm install -g @wispbit/local
```

## Interpreting Results & Handling Dismissed Issues

**No issues:**
Report that no issues were found and continue.

**Issues found:**

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

**Source:** [Link text](https://url-from-cli-output)

---

Which issues would you like me to fix?
````

**URL Sources:** The CLI output may include URL sources for issues (e.g., links to documentation or rule references). If present, always include these URLs in your output so the user can reference them.

When issues are found:
1. Explain each issue to the user
2. Offer to fix the issues
3. After fixes, run `wispbit diff --claude-session-id ${CLAUDE_SESSION_ID}` again to verify

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

By default, reviews use `--claude-session-id` to cover the files changed in the **current Claude session**.

```bash
wispbit diff --claude-session-id ${CLAUDE_SESSION_ID}
```

This command:
1. Gets the git changes from the current session
2. Evaluates changes against Wispbit rules
3. Reports any issues found

Use this mode when the user asks for a review without specifying scope, or after making code changes.

### Committed Files Review (`--committed`)

The `--committed` flag is only available for **non-session reviews** (i.e., `wispbit diff --committed` without `--claude-session-id`). It does **not** work with `--claude-session-id` — session-based reviews always include all files changed during the session regardless.

```bash
wispbit diff --committed
```

Use `--committed` when:
- The user explicitly asks to review committed files or uses `--committed`
- The user wants to check changes that have already been committed locally
- **Do NOT combine with `--claude-session-id`**

## Remembering Feedback

To save general feedback or conventions without dismissing a specific issue, use the standalone `remember` command:

```bash
wispbit remember "feedback or convention to remember"
```

Use this when the user shares preferences or context that should inform future reviews (e.g., "we use snake_case for database columns").

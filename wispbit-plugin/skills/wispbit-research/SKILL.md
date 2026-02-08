---
name: wispbit-research
description: Search your organization's knowledge base for tribal knowledge, documentation, and codebase context. You SHOULD use this proactively before planning implementations or writing code to look up relevant context, conventions, and updated documentation.
version: 1.0.0
allowed-tools: Bash(wispbit:*)
---

# Wispbit Research

Use `wispbit search` to look up tribal knowledge, internal documentation, and codebase context from your organization's knowledge base.

**IMPORTANT**: You SHOULD proactively use this skill:
- **Before planning** — search for relevant context, conventions, or architecture decisions
- **Before writing code** — search for existing patterns, utilities, or documentation on the area you're about to modify
- **When unsure** — search for clarification on naming conventions, preferred libraries, or team standards

## Prerequisites

The `wispbit` CLI must be installed. If a command fails with "command not found", guide the user to install it:

```bash
npm install -g @wispbit/local
```

## Usage

```bash
wispbit search "<query>"
```

### Query Tips

- Use natural language queries describing what you're looking for
- Be specific about the topic, component, or pattern you need context on
- Examples:
  - `wispbit search "authentication flow"`
  - `wispbit search "how to add a new API endpoint"`
  - `wispbit search "database migration conventions"`
  - `wispbit search "error handling patterns"`

## When to Search

1. **Starting a new task** — search for any existing documentation or decisions related to the feature/area
2. **Unfamiliar code area** — search for context before diving into modifications
3. **Architecture questions** — search for design decisions or ADRs
4. **Before proposing an approach** — verify your plan aligns with team conventions

## Interpreting Results

- Use the returned knowledge to inform your implementation approach
- If results reference specific patterns or conventions, follow them
- If no results are found, proceed with standard best practices and note the gap to the user

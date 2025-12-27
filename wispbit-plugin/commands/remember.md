---
allowed-tools: Bash(wispbit:*)
description: Remember a code pattern for future enforcement
---

Remember a code pattern for future enforcement.

Behavior:

- If the user provides arguments (e.g., `/remember Use composition instead of inheritance`), use that as the note
- If the user is discussing code in context, infer what they want to remember from the conversation
- Only prompt for clarification if there's not enough context to understand what should be remembered
- Automatically detect the relevant code selection, file path, and line range from the current context

Steps:

1. Determine the note from command arguments or conversation context
2. Identify the code to remember (from selection, recent edits, or conversation)
3. Get the file path and line range
4. Execute: `wispbit remember <file> --line-start <start> --line-end <end> --note "<note>"`
5. Confirm success to user

Examples:

- `/remember Use composition instead of inheritance` - remembers currently selected/discussed code with that note
- `/remember` (while discussing a code pattern) - infers the pattern and note from context

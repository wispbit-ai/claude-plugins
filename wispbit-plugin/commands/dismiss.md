---
allowed-tools: Bash(wispbit:*)
description: Dismiss wispbit violations
---

Dismiss wispbit code quality violations.

Steps:
1. Run `wispbit diff --json=pretty` to get current violations
2. Parse the output to identify violations
3. Determine which violations the user wants to dismiss based on their request
4. Execute dismiss command with appropriate flags
5. The command will provide feedback on:
   - Whether the rule was found (by ID or internal ID)
   - Whether the file was found (if specified)
   - How many violations were dismissed

Examples:
- "Dismiss all RULE-123 violations" → `wispbit dismiss --rule RULE-123`
- "Dismiss RULE-123 in src/app.ts" → `wispbit dismiss --rule RULE-123 --file src/app.ts`
- "Dismiss the violation on line 10" → `wispbit dismiss --rule RULE-123 --file src/app.ts --line 10`
- "Dismiss violations on lines 10, 22, and 45" → `wispbit dismiss --rule RULE-123 --file src/app.ts --line 10,22,45`

Important:
- Always use relative file paths from the workspace root
- The command will validate that the rule exists (by display ID or internal ID)
- The command will validate that the file exists
- You will receive clear feedback on how many matches were dismissed




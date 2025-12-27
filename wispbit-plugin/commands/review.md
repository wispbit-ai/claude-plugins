---
allowed-tools: Bash(wispbit:*)
description: Run wispbit to check for code quality violations
---

Run wispbit to check for code quality violations in the current workspace.

Execute: `wispbit diff --json=pretty`

The output will be a JSON array of violations. Parse and format them following these rules:

1. **Group by Rule**: Group all issues by their `ruleId` and `message`
2. **No Tables**: Present information in a clean, readable format without tables
3. **Show Code Snippets**: Display the relevant code from the `text` field
4. **Show Fixes**: Display fixes in diff format from the `edits` field. Combine multiple edits for the same file when possible
5. **Ask for Action**: After showing all issues, ask which ones the user would like fixed

**Output Format:**

````
## 🔍 Wispbit Review

Found X violation(s) across Y file(s)

### [RULE-ID] Rule Message

**File:** `path/to/file.ts` (Line X-Y)

```language
code snippet here
````

**Issue:** Description of what's wrong

**Fix Available:**

```diff
- old code
+ new code
```

---

[Repeat for each issue]

---

Which issues would you like me to fix? (Reply with rule IDs or "all")

````

**Example JSON Structure:**
```json
[
  {
    "file": { "path": "src/file.ts" },
    "range": { "start": { "line": 22 }, "end": { "line": 22 } },
    "language": "TypeScript",
    "text": "import { logger } from \"powerlint/utils/logger\"",
    "evidence": [{ "description": "Explanation of the issue" }],
    "edits": [
      {
        "filePath": "src/file.ts",
        "oldString": "old code",
        "newString": "new code"
      }
    ],
    "ruleId": "RULE-123",
    "severity": "violation",
    "message": "Rule description"
  }
]
````

Keep the output clean and condensed so users can quickly understand the issues and decide what to fix.

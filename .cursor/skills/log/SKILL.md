---
name: log
description: Log achievements, view the current quarter's log, or edit existing entries.
---

# Log Skill

## When to use
- "Log that I...", "I shipped...", "add an entry", "/log"
- "Show my logs", "what did I log this quarter", "/logs"
- "Edit my log", "delete an entry", "/edit"

---

## Logging an achievement

**Single entry** — user says "log that I shipped X" or "/log Shipped X":
1. Auto-categorize from the description (see table below)
2. Format: `[YYYY-MM-DD] | {Category} | {Description}`
3. Append to `data/achievements/{YEAR}/Q{N}_log.md` (create with header if missing)
4. Confirm: `✅ Logged: [{date}] | {Category} | {description}`

**Bulk entry** — user types "/log" alone or pastes a list:
Respond: "Paste your entries, one per line starting with `-`"
Process each line as a separate entry, append all at once, confirm the full list.

**If the entry is vague** (e.g. "fixed a bug", "reviewed PRs") — before saving, ask:
"What was the impact or outcome? (e.g. reduced error rate by 30%, unblocked 2 engineers)"
Save with the enriched description.

### Auto-categorization
| Category | Signals |
|----------|---------|
| Achievement | shipped, launched, completed, delivered, released, implemented |
| Challenge | fixed, debugged, resolved, unblocked, recovered, solved |
| Skill Development | learned, completed course, studied, certified, explored |
| Collaboration | mentored, reviewed, paired, helped, led meeting, supported |
| Project Update | progress on, status of, milestone, working on |
| Recognition | award, shoutout, praised, thanked, recognized |

Default: **Achievement**

---

## Viewing logs (`/logs`, "show my logs")

Read `data/achievements/{YEAR}/Q{N}_log.md` and display all entries grouped by category with counts.
If user says `/logs Q2 2025` — read that quarter's file instead.
If file missing: "No log entries for {QUARTER} {YEAR} yet. Start with 'log that I...'"

---

## Editing entries (`/edit`, "edit my log")

Read the log file, display numbered entries, ask which to edit or delete.
Make the change, write the file once, confirm. Don't read it back.

---

## Rules
- One write operation — never rewrite the whole file for a single append
- Don't check company context for logging — just write
- If log file doesn't exist, create it with header: `# Q{N} {YEAR} Achievement Log`

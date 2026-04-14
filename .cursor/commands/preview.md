# Preview Command

Fetch and display all raw data that would go into a report — without generating the report.

---

## Step 1 — Load Configuration

Read `config/company-context.json` once.

**If config missing:** "Run `/setup` first." → **STOP.**

---

## Step 2 — Determine Quarter

Calculate from today's date (see quarter ranges in `.cursorrules`).
If user specified (e.g., `/preview Q4 2025`), use that instead.

---

## Step 3 — Fetch All Data (ONE PASS, all parallel)

Fetch GitHub PRs, Jira Epics, Jira Tickets, and Local Logs using the same queries as the report command.

See `.cursor/commands/report.md` Step 2 for exact query format.

**Fallback for each source:** If empty or errored, show "None found" in that section — do not abort.

---

## Step 4 — Display Preview

```
🔍 REPORT PREVIEW — {QUARTER} {YEAR}  ({MONTHS})

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 LOCAL LOGS  ({N} entries)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[YYYY-MM-DD] | Category | Entry text
...
(show all entries, or first 10 with "…and {N} more" if >10)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💻 GITHUB PRS  ({N} merged)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
• #123: PR title here
• #456: Another PR title
(show all)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 JIRA EPICS  ({N} found, {N} completed)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
• KEY-123: Epic summary  [In Progress]
• KEY-456: Epic summary  [Done]
(show all)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 JIRA TICKETS  ({N} resolved)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
• KEY-789: Ticket summary  [Story]
• KEY-012: Ticket summary  [Bug]
(show first 15 with "…and {N} more" if >15)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Ready to generate the full report? Say "/report" to continue.
```

**→ STOP. Do not auto-generate the report.**

---

## Behavior

- Fetch all data sources once — no retries
- Show raw data as-is — no narrative, no interpretation
- "None found" for empty sources — never abort
- Stop after displaying preview and wait for user

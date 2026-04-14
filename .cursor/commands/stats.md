# Stats Command

Show quick metrics from GitHub and Jira for the current quarter — no narrative, no report.

---

## Step 1 — Load Configuration

Read `config/company-context.json` once. Extract GitHub and Jira credentials.

**If config missing:** "Run `/setup` first." → **STOP.**

---

## Step 2 — Determine Quarter

Calculate from today's date (see quarter ranges in `.cursorrules`).
If user specified a quarter (e.g., `/stats Q3 2025`), use that instead.

---

## Step 3 — Fetch Metrics (ONE PASS, all parallel)

**GitHub PRs:**
```
tool: mcp_github_search_issues
q: "is:pr author:{username} repo:{owner}/{repo} merged:{START}..{END}"
perPage: 100
```

**Jira Epics:**
```
tool: mcp_mcp-atlassian_jira_search
jql: "assignee = \"{jira_email}\" AND issuetype = Epic AND updated >= {START} ORDER BY updated DESC"
limit: 50
```

**Jira Tickets:**
```
tool: mcp_mcp-atlassian_jira_search
jql: "assignee = \"{jira_email}\" AND issuetype IN (Story, Task, Bug) AND resolution IS NOT EMPTY AND updated >= {START} ORDER BY updated DESC"
limit: 100
```

**Local Logs:**
Read `data/achievements/{YEAR}/Q{N}_log.md`.

**Fallbacks:** If any source returns 0 results or errors, show `N/A` for that section — do not abort.

---

## Step 4 — Display Stats

```
📊 {QUARTER} {YEAR} QUICK STATS  ({MONTHS})

💻 GitHub Activity
├─ PRs Merged:   {count}      (unavailable if GitHub errored)
└─ Repos Tracked: {repo list}

📋 Jira Activity
├─ Epics Owned:     {total}
├─ Epics Completed: {completed}
├─ Tickets Resolved: {resolved}
└─ By Type: {N} Stories · {N} Tasks · {N} Bugs

📝 Local Logs
├─ Achievements:    {count}
├─ Challenges:      {count}
├─ Collaborations:  {count}
└─ Total Entries:   {total}

─────────────────────────────
💡 /report → full narrative   /preview → see all raw data
```

**→ STOP. Do not generate a report or ask questions.**

---

## Behavior

- Fetch each source once — no retries
- Show `N/A` for any unavailable source, never abort
- Display stats and stop — no narrative, no follow-up questions

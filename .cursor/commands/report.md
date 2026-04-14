# Report Command

Generate a comprehensive quarterly report with narrative, data evidence, and an HTML file.

---

## Step 0 — Determine Quarter

If the user specified a quarter (e.g., `/report Q3 2025`), use that.
Otherwise, calculate from today's date using the ranges in `.cursorrules`.

Set these values once and use them throughout:
- `QUARTER` = Q1 / Q2 / Q3 / Q4
- `YEAR` = 4-digit year
- `START` = YYYY-MM-DD
- `END` = YYYY-MM-DD
- `MONTHS` = human-readable range (e.g., "January – March")

---

## Step 1 — Load Configuration

Read `config/company-context.json` once.

Extract:
- `github.author`, `github.repositories`
- `user_profile.jira_email`
- `user_profile.name`, `user_profile.role`, `user_profile.team`
- `company.name`, `core_values`, `strategic_priorities`

**If config is missing or not initialized:**
> "Config not found. Run `/setup` first to configure GitHub, Jira, and your profile."
> **STOP.**

---

## Step 2 — Fetch Data (ONE PASS, all parallel)

Run these simultaneously. Each is fetched exactly once — never retry.

**A. Local logs**
Read `data/achievements/{YEAR}/Q{N}_log.md`.
- If file missing or empty → note "No local log entries for this quarter" and continue.

**B. GitHub PRs** (for each tracked repo)
```
tool: mcp_github_search_issues
q: "is:pr author:{github.author} repo:{owner}/{repo} merged:{START}..{END}"
sort: updated
order: desc
perPage: 50
```
- If returns 0 results → note "No merged PRs found for this quarter" and continue.
- If tool errors → note "GitHub data unavailable" and continue.

**C. Jira Epics**
```
tool: mcp_mcp-atlassian_jira_search
jql: "assignee = \"{jira_email}\" AND issuetype = Epic AND updated >= {START} ORDER BY updated DESC"
limit: 20
fields: summary,status,issuetype,priority,updated,created,resolution
```
- If returns 0 results → note "No epics found" and continue.
- If tool errors → note "Jira data unavailable" and continue.

**D. Jira Tickets**
```
tool: mcp_mcp-atlassian_jira_search
jql: "assignee = \"{jira_email}\" AND issuetype IN (Story, Task, Bug) AND resolution IS NOT EMPTY AND updated >= {START} ORDER BY updated DESC"
limit: 50
fields: summary,status,issuetype,priority,updated,resolution
```
- Same fallback: note unavailability and continue.

---

## Step 3 — Show Data Summary, Then Proceed Immediately

Display what was found, then continue directly to report generation — no questions asked.

```
📊 {QUARTER} {YEAR} Data Summary ({MONTHS}):
   • Local Logs:      {N} entries
   • GitHub PRs:      {N} merged
   • Jira Epics:      {N} found ({N} completed)
   • Jira Tickets:    {N} resolved

Generating your report now…
```

---

## Step 4 — Generate Markdown Report

Use this structure. Write in first-person professional prose — not bullet dumps. Derive all content from the fetched data; do not ask the user any questions.

```markdown
# Quarterly Connection Summary: {name} — {QUARTER} {YEAR}

**Role:** {role} | **Team:** {team}
**Period:** {START} – {END}

---

## I. Key Achievements & Accomplishments

{3–5 paragraphs using: What → How it was accomplished → Why it mattered}
Reflect not only on WHAT was accomplished but also on HOW — collaboration, ownership, problem-solving approach.

## II. Q{N} {YEAR} Performance Goals

Individual goals that were set for this quarter, aligned to {company} strategic objectives. Use the S.M.A.R.T. framework (Specific, Measurable, Achievable, Relevant, Timely). Derive 2–3 goals from:
- `career_goals` in config (check if `career_goals.quarter` matches this quarter)
- Epics that were closed/resolved this quarter
- Skills applied or developed this quarter

For each goal, include:
- Goal statement (specific, outcome-focused)
- How success is measured
- Status: Completed / In Progress (reflect what was actually achieved this quarter)
- Due date: last day of this quarter ({END})
- Red Hat strategic alignment

## III. Alignment with {company} Values

Connect specific work from this quarter to {company} core values. Write one paragraph per value (2–4 values). Use concrete examples from PRs and Jira tickets — not generic statements.

## IV. Data-Driven Evidence

### GitHub Contributions — {N} PRs Merged
| # | PR | Title | Merged |
|---|----|-------|--------|
{List all merged PRs with links}

### Jira Epics Owned — {N} Total ({N} Completed)
{List all epics with key, summary, and status}

### Jira Tickets Completed — {N} Resolved
{Top 10–15 resolved tickets with key, summary, type, and status}

---
*Generated {today's date} by Quarterly Connection Strategist*
```

---

## Step 5 — Generate HTML Report

Read `data/templates/quarterly-report-template.html` first. Copy its CSS exactly — do not write custom styles.

The HTML report must include these sections in order:
1. **Header** — name, role, quarter, period
2. **Stats Grid** — PRs merged, epics owned, tickets resolved, epics completed
3. **Key Achievements & Accomplishments** — narrative paragraphs (What → How → Impact)
4. **Q{N} {YEAR} Performance Goals** — 2–3 SMART goal cards showing what was set and accomplished this quarter, with status badge (Completed / In Progress), due date, and strategic alignment tags. Add CSS for `.goal-card`, `.goal-meta`, `.goal-tag`, `.tag-due`, `.tag-aligns`, `.tag-status-completed`, `.tag-status-inprogress` if not already in the template.
5. **Alignment with {company} Values** — value cards with concrete examples
6. **GitHub Contributions** — full PR table with links
7. **Jira Epics & Tickets** — epics grid + resolved tickets grid
8. **Looking Ahead** — purple growth card summarizing Q{NEXT_N} focus

Replace these placeholders:

| Placeholder | Value |
|-------------|-------|
| `{{USER_NAME}}` | user's name |
| `{{USER_TITLE}}` | role |
| `{{QUARTER}}` | Q1 / Q2 / Q3 / Q4 |
| `{{YEAR}}` | year |
| `{{QUARTER_MONTHS}}` | e.g., "January – March" |
| `{{COMPANY_NAME}}` | company name |
| `{{PRS_MERGED}}` | count |
| `{{EPICS_OWNED}}` | count |
| `{{TICKETS_RESOLVED}}` | count |
| `{{EPICS_COMPLETED}}` | count |
| `{{NEXT_QUARTER}}` | e.g., "Q2 2026" |
| `{{GENERATION_DATE}}` | today's date |

Write the output to: `{QUARTER}-{YEAR}-report.html` in the project root.

**Jira card CSS classes:**
- `.epic` = purple border (in-progress epics)
- `.completed` = green border (done/resolved)
- `.status-done` = green badge | `.status-progress` = orange badge | `.status-new` = blue badge

---

## Step 6 — Confirm and Stop

```
✅ Report generated.

📄 Markdown: above
📄 HTML: {QUARTER}-{YEAR}-report.html

Open the HTML file in a browser to review, or say "export" for PDF instructions.
```

**→ STOP. Wait for user.**

---

## Behavior Summary

- Read each data source exactly once — no retries, no verification reads
- If any data source is empty or unavailable, note it and continue (never abort)
- **Never ask reflection questions** — derive all report content from fetched data and config
- SMART goals reflect **this quarter's goals** (from `career_goals` in config + closed epics) — show what was set and what was achieved, not forward-looking Q+1 plans
- Stop after Step 6 — do not suggest follow-ups automatically

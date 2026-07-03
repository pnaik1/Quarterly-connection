# Quarterly Connection — Constitution

A personal quarterly review tool. Captures your GitHub PRs, Jira tickets, and logged achievements into a polished HTML report aligned with Red Hat's competency framework.

---

## Files and what they are

| File | Purpose | Committed? |
|------|---------|------------|
| `config/company-context.json` | Your profile, GitHub username, Jira email, goals | No — gitignored |
| `config/company-context.template.json` | Blank starting point for new users | Yes |
| `config/competencies.json` | Red Hat competency framework (8 categories, 35+ sub-competencies) | Yes |
| `data/achievements/{YEAR}/Q{N}_log.md` | Your manual achievement log | No — gitignored |
| `data/templates/quarterly-report-template.html` | HTML report template — CSS lives here | Yes |
| `*-report.html` | Generated HTML reports | No — gitignored |
| `~/.cursor/projects/{workspace}/canvases/*-quarterly-report.canvas.tsx` | Interactive canvas report (Cursor side panel) | No — outside workspace |

---

## Quarter ranges

| Quarter | Months | Start | End |
|---------|--------|-------|-----|
| Q1 | January – March | Jan 1 | Mar 31 |
| Q2 | April – June | Apr 1 | Jun 30 |
| Q3 | July – September | Jul 1 | Sep 30 |
| Q4 | October – December | Oct 1 | Dec 31 |

---

## Log entry format

```
[YYYY-MM-DD] | {Category} | {Description}
```

**Categories:** Achievement · Challenge · Skill Development · Collaboration · Project Update · Recognition

Log file path: `data/achievements/{YEAR}/Q{N}_log.md`

---

## Data queries

**GitHub PRs (per tracked repo):**
```
is:pr author:{github.author} repo:{owner}/{repo} merged:{START}..{END}
```

**Jira Epics:**
```
assignee = "{jira_email}" AND issuetype = Epic AND updated >= {START} ORDER BY updated DESC
```

**Jira STRATs (derived from epics):**
For each epic, fetch the issue detail and check `parent` for a `RHAISTRAT-*` key. Collect unique RHAISTRAT features — only include STRATs whose child epics were worked on this quarter.

**Jira Tickets:**
```
assignee = "{jira_email}" AND issuetype IN (Story, Task, Bug) AND resolution IS NOT EMPTY AND updated >= {START} ORDER BY updated DESC
```

---

## Report output

| Output | Location | Description |
|--------|----------|-------------|
| Canvas | `~/.cursor/projects/{workspace}/canvases/{q}{N}-{YEAR}-quarterly-report.canvas.tsx` | Interactive Cursor side panel — tabs for achievements, goals, competencies, and data |

## Report sections (in order)

1. **Key Achievements & Accomplishments** — What → How → Why it mattered
2. **Performance Goals** — Goals linked to parent RHAISTRAT features, with bulleted Jira-linked progress (no strategic alignment, no vague statements)
3. **Alignment with Red Hat Values** — Concrete examples per value from PRs/tickets
4. **Red Hat Competency Alignment** — 4–6 sub-competencies with evidence from `config/competencies.json`
5. **Data-Driven Evidence** — STRAT features table + full PR table + Jira epics + resolved tickets

---

## Dependencies

- **Cursor** with **GitHub MCP** and **Atlassian MCP** configured
- Without MCP: logging and manual reporting still work — no auto-fetched data

---

## For teams

Everyone uses their own `config/company-context.json` (gitignored). Shared files — `competencies.json`, `company-context.template.json`, templates, and all skills — are committed. New user: clone → open in Cursor → "set me up."

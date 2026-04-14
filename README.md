# Quarterly Connection Strategist

An AI-powered quarterly review system that lives inside Cursor. Log your work naturally throughout the quarter, then generate polished, data-backed reports by pulling directly from GitHub and Jira.

---

## Prerequisites

Before using this repo, you need:

1. **[Cursor](https://cursor.sh)** — this is a Cursor-native project (not plain VS Code)
2. **MCP: GitHub** — for fetching merged PRs ([setup guide](https://docs.cursor.com/context/model-context-protocol))
3. **MCP: Atlassian** — for fetching Jira tickets ([setup guide](https://docs.cursor.com/context/model-context-protocol))

> Without GitHub/Jira MCP tools, logging and manual reporting still work — you just won't get auto-fetched data.

---

## Quick Start

### Step 1 — Open in Cursor

Clone or download this repo and open the folder in Cursor (File → Open Folder).

### Step 2 — Configure

In the Cursor chat panel, type:

```
/setup
```

The agent walks you through:
- Company name (auto-fetches values from the web)
- Your name, role, and team
- GitHub username and repositories to track
- Jira email and project keys

All saved to `config/company-context.json`.

### Step 3 — Set Goals

```
/goals set
```

Define what you want to accomplish this quarter. Stored alongside your config.

### Step 4 — Log Throughout the Quarter

Capture wins as they happen — don't wait until report time:

```
/log Shipped new API gateway, reducing response times by 45%
/log [Challenge]: Debugged race condition in payment system affecting prod
/log [Collaboration]: Mentored two engineers on testing patterns
```

The AI auto-categorizes entries and saves them to `data/achievements/{YEAR}/Q{N}_log.md`.

### Step 5 — Generate Your Report

At quarter end:

```
/report
```

The agent fetches your GitHub PRs and Jira tickets, combines them with your logs, asks three reflection questions, then writes a polished Markdown + HTML report.

---

## All Commands

Commands work with or without the `/` prefix.

### Setup & Profile

| Command | What it does |
|---------|-------------|
| `/setup` | Configure company, GitHub, Jira, and your profile |
| `/profile` | Update your name, role, or team |
| `/status` | Show current quarter dates and log entry counts |
| `/goals` | View career goals for this quarter |
| `/goals set` | Set new short-term and long-term goals |
| `/help` | Show this command list inside the chat |

### Logging

| Command | What it does |
|---------|-------------|
| `/log <text>` | Add an achievement (auto-categorized) |
| `/log [Category]: <text>` | Add with a specific category |
| `/logs` | View this quarter's entries |
| `/logs Q2 2025` | View a specific quarter's entries |
| `/edit` | Edit or delete existing entries |

**Log categories:** Achievement · Challenge · Skill Development · Collaboration · Project Update · Recognition

### Reporting

| Command | What it does |
|---------|-------------|
| `/preview` | Fetch all data and show it — without generating the report |
| `/stats` | Quick metrics only (no narrative) |
| `/report` | Full report for the current quarter |
| `/report Q3 2025` | Full report for a specific quarter |
| `/compare Q3 Q4` | Side-by-side metric comparison |
| `/export` | Instructions for saving the HTML report as PDF |

---

## Report Flow

```
/report
   │
   ├─ Step 1: Load config (company, GitHub, Jira credentials)
   │
   ├─ Step 2: Fetch data in parallel
   │           ├─ GitHub: merged PRs by author + date range
   │           ├─ Jira: epics and resolved tickets
   │           └─ Local: achievement log entries
   │
   ├─ Step 3: Show data summary ──► STOP, ask 3 reflection questions:
   │           1. Which company values did you exemplify?
   │           2. What was your biggest impact?
   │           3. What skill do you want to develop next quarter?
   │
   ├─ Step 4: Generate Markdown report (after you answer)
   │
   └─ Step 5: Generate HTML report → {QUARTER}-{YEAR}-report.html
```

The HTML report is print-optimized. Open it in Chrome and use Cmd+P → Save as PDF.

---

## Project Structure

```
Quarterly-connection/
├── .cursorrules                      # Agent persona + command routing (95 lines)
├── .cursor/
│   └── commands/                     # One file per command — single source of truth
│       ├── report.md
│       ├── log.md
│       ├── setup.md
│       ├── stats.md
│       ├── preview.md
│       └── ... (13 commands total)
├── config/
│   └── company-context.json          # Your profile, company values, GitHub/Jira creds
├── data/
│   ├── achievements/
│   │   ├── 2025/                     # Q4_log.md etc.
│   │   └── 2026/                     # Q1_log.md etc.
│   └── templates/
│       ├── quarterly-report-template.md
│       └── quarterly-report-template.html
├── Q1-2026-report.html               # Generated — open in browser
└── .gitignore
```

---

## How the Agent Works

This project is structured as a **3-layer Cursor agent**:

```
.cursorrules  (95 lines)
   Persona, rules, and a routing table.
   No command logic lives here.
      │
      ▼
.cursor/commands/*.md
   One file per command. Each file owns its full behavior:
   steps, error handling, and an explicit STOP signal.
      │
      ▼
MCP Tools (GitHub + Jira)
   Called once per session when a command requires it.
   Results are never re-fetched to "verify."
```

### Why this structure

- **`.cursorrules` is short on purpose.** The longer the system prompt, the less reliably the AI follows instructions at the end. At 95 lines it stays fully in context.
- **Commands are self-contained.** Updating `/report` means editing one file, not hunting through a 700-line rules file.
- **Error handling is explicit.** Every command specifies what to do when GitHub returns 0 results, Jira is unreachable, or a log file doesn't exist yet — never silently fails, never aborts.

---

## Tips for Better Reports

**Log often, not at the end of the quarter.**
Five minutes after you ship something is the best time to log it. At report time, the details will have faded.

**Be specific about impact.**

| Instead of | Write |
|-----------|-------|
| `Fixed a bug` | `Fixed auth bug affecting 10K users, reduced support tickets by 60%` |
| `Reviewed PRs` | `Reviewed 8 PRs for new feature, caught 2 breaking API changes before merge` |
| `Attended meeting` | `Led architecture review for BFF layer, unblocked 3 downstream teams` |

**Use the right category.** The auto-categorizer is good, but explicit categories make filtering cleaner:
```
/log [Skill Development]: Completed Golang fundamentals course
/log [Recognition]: Team shoutout for leading production incident response
```

---

## Recommended Workflow

**Start of quarter (30 min)**
```
/goals set       ← What do you want to accomplish?
```

**Weekly (5 min)**
```
/log <your wins this week>
/status          ← See how many entries you have
```

**Mid-quarter (15 min)**
```
/stats           ← Check GitHub/Jira numbers
/goals           ← Review progress against goals
```

**End of quarter (30 min)**
```
/preview         ← See all data before writing
/report          ← Generate the full report
/export          ← Save as PDF for your manager
```

---

## Customization

**Add a new command:** Create `.cursor/commands/your-command.md` with steps, error handling, and a STOP signal at the end. Add a row to the routing table in `.cursorrules`.

**Change the report design:** Edit `data/templates/quarterly-report-template.html`. The CSS variables at the top control all colors and fonts.

**Track multiple repos:** Update `config/company-context.json` → `github.repositories` with additional `{ "owner": "...", "repo": "..." }` entries.

**Use with a different company:** Run `/setup` again — it overwrites the config and fetches new company values.

---

## Philosophy

- **Evidence over assertion** — every claim in the report is backed by a PR, ticket, or log entry
- **Consistent beats perfect** — a quick `/log` today is worth more than a thorough one next month
- **Local and private** — all data lives in markdown files on your machine
- **AI augments, you narrate** — the agent gathers and structures; you provide the reflection

---

*Built with Cursor AI + MCP Tools · MIT License*

# Quarterly Connection

An AI-powered quarterly review tool. Log your work as you go, then generate a polished interactive report backed by real GitHub PRs, Jira tickets, and Slack contributions — including Red Hat competency alignment.

---

## Prerequisites

1. **[Cursor](https://cursor.sh)**
2. **GitHub MCP** — for fetching merged PRs
3. **Atlassian MCP** — for fetching Jira tickets
4. **Slack MCP** *(optional)* — for extracting Slack contributions into the report

> Any MCP that is unavailable is skipped gracefully — the report is still generated with whatever sources are available.

### Slack MCP setup

The Slack integration uses [slack-mcp](https://github.com/redhat-community-ai-tools/slack-mcp). Run the one-time setup script:

```bash
python3 <(curl -fsSL https://raw.githubusercontent.com/redhat-community-ai-tools/slack-mcp/main/scripts/setup-slack-mcp.py)
```

Once connected, add the channels you want tracked to `config/company-context.json` under the `slack.channels` key (see [Slack channel config](#slack-channel-config) below). This is a one-time manual step — every person's relevant channels are different.

---

## Quick start

```bash
git clone <this-repo>
# Open in Cursor, then in the chat panel say:
"set me up"
```

The setup skill walks you through 4 questions and creates your personal config.

---

## How to use it

Just talk to it naturally. Cursor matches your intent to the right skill.

| You say | What happens |
|---------|-------------|
| "set me up" / "update my GitHub" | Setup skill — configure profile, GitHub, Jira |
| "log that I shipped X" | Log skill — adds an entry to this quarter's log |
| "show my logs" | Log skill — displays current quarter's entries |
| "set my goals for Q2" | Goals skill — saves short-term, long-term, skills |
| "generate my Q2 report" | Report skill — fetches all data sources, builds interactive canvas |
| "preview my data" | Report skill — shows raw data without generating |
| "quick stats" | Report skill — PR/Jira/Slack counts only |
| "compare Q1 and Q2" | Report skill — side-by-side metrics |
| "show my competency alignment" | Competencies skill — maps work to Red Hat framework |

---

## Data sources

The report pulls from four sources in parallel, each fetched exactly once per generation:

| Source | What it captures | Requires |
|--------|-----------------|----------|
| **GitHub** | Merged PRs across tracked repos | GitHub MCP or `gh` CLI |
| **Jira** | Closed epics, resolved stories, tasks, bugs | Atlassian MCP |
| **Slack** | Architecture proposals, cross-team coordination, documentation, knowledge-sharing from tracked channels | Slack MCP + channel config |
| **Local log** | Manually logged achievements | Nothing — always available |

### How Slack extraction works

1. The report skill reads `slack.channels` from your config to know which channels to pull.
2. For each channel, it calls `get_channel_history` filtered to the quarter date window.
3. It keeps only messages authored by you, discarding other people's messages.
4. The AI synthesizes raw messages into structured highlights — filtering out casual chat, grouping related messages in a thread, and classifying each contribution: **Architecture**, **Coordination**, **Documentation**, **Knowledge-sharing**, or **Cross-team**.
5. Highlights are embedded into the canvas and woven throughout **Achievements**, **Values**, **Goals**, and **Competencies** — not just a dedicated Slack tab.

---

## Workflow

**Start of quarter (10 min)**
1. Open Cursor in this folder
2. "Set my goals for Q{N}"
3. Add any new relevant Slack channels to the `slack.channels` config

**Weekly (2 min)**
- "Log that I..." — capture wins while they're fresh

**End of quarter (15 min)**
1. "Preview my data for Q{N}" — see what all sources have
2. "Generate my Q{N} report" — builds an interactive canvas with all your data
3. Click the canvas link to open it beside the chat

---

## Project structure

```
Quarterly-connection/
├── CONSTITUTION.md                   # data model, queries, report structure
├── CLAUDE.md                         # Claude Code bridge (for non-Cursor users)
├── .cursor/
│   ├── rules/
│   │   └── quarterly-connection.mdc  # always-on session rules
│   └── skills/
│       ├── setup/SKILL.md            # profile + config
│       ├── log/SKILL.md              # logging, viewing, editing
│       ├── report/SKILL.md           # preview, stats, report, compare, export
│       ├── goals/SKILL.md            # view and set goals
│       └── competencies/SKILL.md     # Red Hat competency alignment
├── config/
│   ├── company-context.json          ← gitignored (your credentials + goals)
│   ├── company-context.template.json ← blank starting point
│   └── competencies.json             ← Red Hat framework (shared)
├── data/
│   └── achievements/2026/            ← your log files (gitignored)
└── .gitignore
```

---

## Slack channel config

In `config/company-context.json`, add a `slack` block listing the channels you want the report to pull from. These should be team channels, working groups, and project forums — not DMs.

```json
"slack": {
  "channels": [
    { "id": "C09S4J8TV5Y", "name": "team-dashboard-crimson",       "purpose": "team standup and weekly work updates" },
    { "id": "C099MEPGF43", "name": "wg-dashboard-crimson",          "purpose": "dashboard working group technical discussions" },
    { "id": "C07CQ4R4HMY", "name": "forum-openshift-ai-dashboard",  "purpose": "public dashboard forum and announcements" }
  ]
}
```

To find a channel ID: right-click the channel name in Slack → **Copy link** — the ID is the last segment (starts with `C`).

To add a channel mid-quarter: edit the config and re-run the report — previous quarters aren't affected.

---

## For teams

**What's shared (committed):** All skills, templates, `competencies.json`, `company-context.template.json`

**What's personal (gitignored):** `config/company-context.json`, `data/achievements/`

New teammate onboarding:
```bash
git clone <this-repo>
# Open in Cursor → "set me up"
# Then add your Slack channels to config/company-context.json
```

**Using Claude Code instead of Cursor?** See `CLAUDE.md`.

---

## Tips for better reports

Log often — five minutes after you ship something is the best time.

| Instead of | Write |
|-----------|-------|
| "Fixed a bug" | "Fixed auth bug affecting 10K users, reduced support tickets by 60%" |
| "Reviewed PRs" | "Reviewed 8 PRs for new feature, caught 2 breaking API changes before merge" |
| "Attended meeting" | "Led architecture review for BFF layer, unblocked 3 downstream teams" |

For Slack: the report picks up substantive messages automatically, but the best channels to track are ones where you share technical updates, ask architectural questions, or coordinate cross-team work. Pure social channels don't add value.

---

*Built with Cursor AI + MCP Tools · MIT License*

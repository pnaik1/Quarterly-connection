---
name: report
description: Preview data, generate stats, create the quarterly canvas report, or compare quarters.
---

# Report Skill

## When to use
- "Generate my report", "create Q2 report", "/report"
- "Preview my data", "what's in my data", "/preview"
- "Quick stats", "how many PRs", "/stats"
- "Compare Q1 and Q2", "/compare Q1 Q2"

---

## Determine the quarter

From the user's message (e.g. "Q2 2026") or today's date. Set `QUARTER`, `YEAR`, `START` (YYYY-MM-DD), `END` (YYYY-MM-DD).

---

## Load config

Read `config/company-context.json` once. Also read `config/competencies.json` once.
If config missing or not initialized → "Run setup first." Stop.

---

## Fetch data (all in parallel, each source exactly once)

| Source | Query |
|--------|-------|
| Local logs | Read `data/achievements/{YEAR}/Q{N}_log.md` |
| GitHub PRs | `is:pr author:{github.author} repo:{owner}/{repo} merged:{START}..{END}` — run for each tracked repo |
| Jira Epics | `assignee = "{jira_email}" AND issuetype = Epic AND updated >= {START} ORDER BY updated DESC` |
| Jira Tickets | `assignee = "{jira_email}" AND issuetype IN (Story, Task, Bug) AND resolution IS NOT EMPTY AND updated >= {START} ORDER BY updated DESC` |
| Jira STRATs | For each epic returned, fetch the issue detail and check `parent` for a RHAISTRAT key. Collect unique RHAISTRAT features — these are the STRATs you led. Only include STRATs whose child epics were worked on this quarter. |
| Slack | For each channel in `config.slack.channels`: call `get_channel_history` with `channel_id`, `oldest: START`, `latest: END`, `include_threads: false`. Collect only messages authored by the user (`from:pnaik` or matching `user_profile.name`). Skip if `config.slack` is missing or Slack MCP is unavailable. |
| AI Tools & Innovation | Search Slack (`from:{user} AI tooling innovation Team Home agent cursor automation after:{START}`) and Jira for any AI tools adoption, automation built, or innovation work. This is a required search — always check. |

If any source errors or returns empty — note it and continue.

---

## Verify then generate — in one turn

After fetching, show a data receipt immediately followed by the canvas. Do not pause and wait for confirmation.

```
Data fetched for {QUARTER} {YEAR}:

  GitHub PRs:      {N} merged  (repos: {repo list})
  STRATs Led:      {N} features (via epic parent links)
  Jira Epics:      {N} found
  Jira Tickets:    {N} resolved
  Log entries:     {N} entries
  Slack messages:  {N} from {M} channels  (or "unavailable" if skipped)

Building canvas...
```

Then proceed directly to canvas generation in the same response.

**If any source returned 0 results**, flag it and stop — do not generate:
```
  GitHub PRs: 0 — check github.author and repo names in config
  Jira Tickets: 0 — verify jira_email matches your Jira login

Fix the config and try again.
```

---

## Preview (`/preview`, "show me the data")

Display all fetched data as-is — no narrative. Show counts and raw items. End with "Say 'generate' to continue." Stop.

## Stats (`/stats`, "quick metrics")

Show just the counts: PRs merged, epics owned, tickets resolved, log entries. Stop.

## Compare (`/compare Q1 Q2`)

Fetch counts for both quarters. Show a side-by-side table with % change. Note the biggest improvement. Stop.

---

## Canvas generation

Write a live `.canvas.tsx` file with all fetched data embedded inline.

**Canvas path:**
- Take the absolute workspace path (e.g. `/Users/pnaik/Desktop/Quarterly-connection`)
- Replace every `/` (except the leading one) with `-`, drop the leading `/`
- Result dir: `~/.cursor/projects/Users-pnaik-Desktop-Quarterly-connection/canvases/`
- File: `{q}{N}-{YEAR}-quarterly-report.canvas.tsx` (all lowercase, e.g. `q2-2026-quarterly-report.canvas.tsx`)

**Canvas file rules:**
- Import only from `cursor/canvas` — no fetch(), no network, no relative imports
- Default-export the top-level component
- All data as `const` arrays at the top — never invent data not present in fetched results
- Set `const IS_SAMPLE_DATA = false`

**Structure:** Use the template canvas at `~/.cursor/projects/Users-pnaik-Desktop-Quarterly-connection/canvases/q2-2026-quarterly-report.canvas.tsx` — copy the full component code verbatim, replace only the data constants:

```tsx
const IS_SAMPLE_DATA = false;

const PROFILE = {
  name: "{user_profile.name}",
  team: "{user_profile.team}",
  quarter: "{QUARTER} {YEAR}",
  period: "{month} 1 – {month} {last}, {YEAR}",
  generated: "{today}",
};

const STATS = [
  { value: {pr_count},     label: "PRs Merged" },
  { value: {strat_count},  label: "STRATs Led" },
  { value: {epic_count},   label: "Epics Owned" },
  { value: {ticket_count}, label: "Tickets Resolved" },
  { value: {slack_count},  label: "Slack Highlights" },
];
// Stats grid uses columns={5}.

// ACHIEVEMENTS — 2–5 items derived from PR titles + log entries. Never invent.
// Each: { title: string, body: string }
// REQUIRED: Always search Slack and Jira for AI tools/innovation work done that quarter
// (e.g. AI tooling adoption, automation tools built, agent/LLM experiments, Team Home usage,
// strat-creator skills, Quarterly Connection tool). If found, include as an achievement
// titled "AI Tools Adoption & Innovation" and also add a corresponding performance goal.

// RED_HAT_VALUES — 2–3 items with concrete examples from real PRs/tickets.
// Each: { name: string, example: string }

// GOALS — Performance goals from career_goals in config + closed epics + STRAT features.
// Structure follows the Red Hat performance review template: Goal title → parent STRAT link → bulleted progress with Jira links.
// Each: {
//   title: string,
//   status: "Completed" | "In Progress",
//   due: string,
//   strat?: { key: string, summary: string, status: string },  // parent RHAISTRAT feature — only for goals that map to a STRAT
//   progressHeader: string,  // e.g. "Epics led and successfully delivered:"
//   progressItems: { key: string, description: string, subItems?: string[] }[]  // Jira-linked items with descriptions
// }
// Rules:
// - Link the parent RHAISTRAT feature via the `strat` field. Find it from the epic's `parent` in Jira.
// - Only include STRATs whose child epics were actually worked on this quarter.
// - Each progressItem `key` is a Jira epic/ticket key rendered as a clickable link.
// - `description` should be concrete — what was built/delivered, with PR numbers where relevant.
// - Do NOT include vague/opaque statements. Every bullet must cite real work.
// - Do NOT include a "strategic alignment" field.

// STRAT_FEATURES — RHAISTRAT features you led this quarter (derived from epic parent links).
// Only include STRATs whose child epics were worked on in the quarter — do NOT include older STRATs.
// Each: { key: string, summary: string, type: string, status: string }

// COMPETENCIES — 4–6 most evidenced from config/competencies.json. Evidence must cite real PR titles or ticket IDs.
// Each: { category, name, definition, evidence, indicators: string[] }

// PRS — full list of merged PRs. Each: { number, title, merged, url }

// JIRA_EPICS — all returned. Each: { key, summary, status }

// JIRA_TICKETS — top 15. Each: { key, summary, type, status }

// SLACK_HIGHLIGHTS — up to 8 substantive contributions from the tracked channels.
// Only include messages that show technical work, decisions, cross-team coordination,
// or knowledge-sharing. Exclude casual chat, greetings, emoji-only, and DMs.
// Synthesize multi-message threads into a single highlight where useful.
// Each: { channel: string, type: "Architecture"|"Coordination"|"Documentation"|"Knowledge-sharing"|"Cross-team", summary: string, date: string }
// If Slack was unavailable, set to []
```

Write the complete file in one pass — do not truncate.

---

## Confirm and stop

```
✅ {QUARTER} {YEAR} canvas ready.

  Open it beside the chat: [{QUARTER} {YEAR} Report]({absolute-canvas-path})

  Tabs: Achievements & Values · Goals · Competencies · Data · Slack
```

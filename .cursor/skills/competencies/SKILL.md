---
name: competencies
description: Map this quarter's GitHub PRs, Jira tickets, and log entries to the Red Hat competency framework. Show what's strongly demonstrated and where to grow.
---

# Competencies Skill

## When to use
- "Show my competency alignment", "which competencies did I demonstrate", "/competencies"
- Can be run standalone or is called automatically during report generation

---

## What to do

**Load (all at once):**
- `config/company-context.json` — name, role, job_level, github, jira_email
- `config/competencies.json` — full competency framework
- `data/achievements/{YEAR}/Q{N}_log.md` — log entries
- GitHub PRs and Jira tickets using queries from CONSTITUTION.md

**Map to competencies:**

For each of the 35+ sub-competencies in `config/competencies.json`, assess evidence strength:
- **Strong** — multiple concrete examples from PRs, tickets, or logs
- **Demonstrated** — at least one clear example
- **Not evidenced** — nothing this quarter

Select the **top 6–8** with the strongest evidence.

**Mapping heuristics** (from CONSTITUTION.md report section):
- PRs merged, features shipped → Execution › Take Initiative, Focus on Results
- Code reviews, PR comments → Team Advocate › Share Feedback; Red Hat Multiplier › Collaborate
- New tech, spikes, learning → Continuous Learning › Experiment, Practice
- Cross-team integrations → Red Hat Multiplier › Connect; Strategic › Navigate Complex Connections
- Bug root-cause analysis → Problem Solving › Troubleshoot, Think Critically
- Customer-facing UI/UX work → Customer Focus › Adopt Customer Perspective
- Architecture, design proposals → Strategic › Generate a Vision

---

## Output format

```
🧭 RED HAT COMPETENCY ALIGNMENT — {QUARTER} {YEAR}
{name} · {role}{job_level ? " · " + job_level : ""}

STRONGLY DEMONSTRATED
▸ {Category} › {Sub-competency}
  "{Definition}"
  Evidence: {1–2 sentences citing specific PRs or tickets}
  Matched indicators: {indicator 1} · {indicator 2}

▸ ...

ALSO DEMONSTRATED
▸ {Category} › {Sub-competency}
  Evidence: {1 sentence}

GROWTH AREAS (not evidenced this quarter)
• {Sub-competency} — one practical suggestion for next quarter
• ...
```

If `job_level` is set, add:
```
📊 JOB LEVEL CONTEXT ({job_level})
2–3 sentences: which competencies align with this level's expectations and
which growth areas could strengthen the case for the next level.
```

Confirm: "✅ Competency alignment complete for {QUARTER} {YEAR}. This section is also included automatically in /report."

---

## Rules
- Only include sub-competencies with real evidence — never pad
- Growth areas: pick 2–3 relevant to the user's role, frame positively
- If `config/competencies.json` is missing: "competencies.json not found in config/. Check the repo." Stop.

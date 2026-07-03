---
name: setup
description: Set up or update the user's Quarterly Connection profile — name, role, team, GitHub, and Jira.
---

# Setup Skill

## When to use
- First time a user opens this repo
- "Set me up", "update my profile", "change my GitHub", "/setup"

## What to do

### If `config/company-context.json` is missing
Welcome them — this is expected on a fresh clone:

```
Welcome to Quarterly Connection!
Your personal config is gitignored — credentials stay on your machine.
3 quick questions and you're done.
```

### If config exists and `initialized: true`
Show current settings and ask what to update in one message:
```
Current config:
   Name:    {name}
   Team:    {team}
   GitHub:  {username} → {repos}
   Jira:    {email}

What needs updating? Just tell me what changed.
```
Parse their reply and update only the fields they mention.

---

## Ask everything at once

Send one message with all questions:

```
A few quick questions:

   • Full name?
   • Team?
   • GitHub username?
   • Repos to track? (format: owner/repo — comma-separate for multiple)
   • Jira email?
```

Parse all answers from one reply. If the user answers in any order or format, extract what you need.

---

## Save to `config/company-context.json`

Red Hat is hardcoded — no company question needed.

```json
{
  "company": {
    "name": "Red Hat",
    "initialized": true,
    "last_updated": "YYYY-MM-DD"
  },
  "core_values": [
    { "name": "Freedom", "description": "Fostering independent thinking and innovation" },
    { "name": "Courage", "description": "Speaking up, taking risks, and challenging the status quo" },
    { "name": "Commitment", "description": "Delivering on promises to customers, partners, and each other" },
    { "name": "Accountability", "description": "Owning outcomes, not just activities" }
  ],
  "strategic_priorities": {
    "year": 2026,
    "priorities": [
      "Hybrid Cloud Platforms Acceleration",
      "Enabling Customer AI Innovation",
      "Operational Excellence"
    ]
  },
  "user_profile": {
    "name": "...",
    "role": "...",
    "job_level": null,
    "team": "...",
    "jira_email": "..."
  },
  "github": {
    "author": "...",
    "repositories": [{ "owner": "...", "repo": "..." }]
  },
  "career_goals": {
    "short_term": [],
    "long_term": [],
    "skills_to_develop": [],
    "quarter": ""
  }
}
```

## Confirm and stop
```
You're all set!
   Profile: {name} · {role} · {team}
   GitHub:  {username} tracking {N} repo(s)
   Jira:    {email}

Next: say "set my goals" to add your Q{N} goals, or "log that I..." to start capturing wins.
```

## Rules
- Never use real usernames or emails as prompt examples — generic placeholders only (e.g. "your-username", "org/repo-name")
- Parse job level from role if present ("Senior Software Engineer" → role: Software Engineer, job_level: Senior) — otherwise leave job_level as null
- Save config once — don't read it back to verify

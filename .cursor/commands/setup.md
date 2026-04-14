# Setup Command

Initialize or update the Quarterly Connection Strategist configuration.

---

## Step 1 — Check Existing Configuration

Read `config/company-context.json`.

- If it exists and `initialized: true`, display current settings:
  ```
  ⚙️ CURRENT CONFIGURATION

  Company:  {name}
  Profile:  {name} · {role} · {team}
  GitHub:   {username} tracking {repo list}
  Jira:     {email} · Projects: {project list}

  What would you like to update? (company / profile / github / jira / all)
  ```
  **→ STOP. Wait for user's choice before proceeding.**

- If missing or `initialized: false`, say "No config found — let's set it up." and go to Step 2.

---

## Step 2 — Gather Information (One Question at a Time)

Ask each question and wait for the answer before asking the next.

**Company:**
```
🏢 What company do you work for?
```
After their answer → do ONE web search: `{company} company values mission`
Show the values you found and ask: "Does this look right, or would you like to adjust anything?"
Wait for confirmation before continuing.

**Profile:**
```
👤 What is your full name?
```
Wait → then:
```
📋 What is your role/title?
```
Wait → then:
```
🏠 What team are you on?
```

**GitHub:**
```
💻 What is your GitHub username? (used to filter PRs — e.g., "pnaik1")
```
Wait → then:
```
📦 Which repositories should I track?
   Format: owner/repo — you can list multiple separated by commas.
   Example: opendatahub-io/odh-dashboard
```

**Jira:**
```
📧 What is your Jira email address?
```
Wait → then:
```
🎫 Which Jira project keys should I search? (optional — e.g., "RHOAIENG, RHAIENG")
   Press Enter to skip.
```

---

## Step 3 — Save Configuration

Write `config/company-context.json` with this structure:

```json
{
  "company": {
    "name": "{company_name}",
    "initialized": true,
    "last_updated": "{today YYYY-MM-DD}"
  },
  "core_values": [
    { "name": "Value Name", "description": "Description from web search" }
  ],
  "strategic_priorities": {
    "year": {current year},
    "priorities": ["Priority 1", "Priority 2"]
  },
  "user_profile": {
    "name": "{name}",
    "role": "{role}",
    "team": "{team}",
    "jira_email": "{email}",
    "jira_projects": ["{PROJECT1}", "{PROJECT2}"]
  },
  "github": {
    "author": "{username}",
    "repositories": [
      { "owner": "{owner}", "repo": "{repo}" }
    ]
  }
}
```

---

## Step 4 — Confirm and Stop

```
✅ Setup complete!

📊 Now tracking:
   Company:  {company_name}
   GitHub:   {username} → {repo list}
   Jira:     {email} → {project list}
   Profile:  {name} · {role} · {team}

Try these next:
   /log    — Add your first achievement
   /status — See current quarter info
   /report — Generate your quarterly report
```

**→ STOP. Do not automatically run any other command.**

---

## Behavior

- Ask questions one at a time — never dump all questions at once
- Do exactly one web search for company values
- Save configuration once — don't re-read to verify
- On partial update (user chose "github" only), update only that section, preserve the rest

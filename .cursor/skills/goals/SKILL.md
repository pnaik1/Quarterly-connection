---
name: goals
description: View or set career goals for the current quarter — short-term, long-term, and skills to develop.
---

# Goals Skill

## When to use
- "What are my goals", "show goals", "/goals"
- "Set my goals", "update my goals for Q2", "/goals set"

---

## Viewing goals

Read `config/company-context.json`. Display `career_goals`:

```
🎯 Goals — {career_goals.quarter}

Short-term (this quarter):
• {goal 1}
• {goal 2}

Long-term:
• {goal 1}

Skills to develop:
• {skill 1}
• {skill 2}
```

If `career_goals.quarter` doesn't match the current quarter, add:
```
⚠️  These goals are from {career_goals.quarter}. Update them with "set my goals."
```

If no goals set: "No goals set yet. Say 'set my goals' to add them."

---

## Setting goals

Ask three questions, one at a time:

1. "What are your 2–3 goals for this quarter?"
2. "Any long-term goals for this year?"
3. "What skills do you want to develop?"

Save to `config/company-context.json` under `career_goals`:
```json
{
  "short_term": ["goal 1", "goal 2"],
  "long_term": ["goal 1"],
  "skills_to_develop": ["skill 1", "skill 2"],
  "quarter": "Q{N} {YEAR}"
}
```

Confirm:
```
✅ Goals saved for Q{N} {YEAR}.
These will appear in your quarterly report automatically.
```

---

## Rules
- View: read config once, display, stop
- Set: ask questions one at a time, save once, stop

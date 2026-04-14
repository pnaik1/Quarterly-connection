# Goals Command

View and set career goals.

## Instructions

### `/goals` - View Goals

Read `config/company-context.json` and display career_goals section:

```
🎯 CAREER GOALS - Q{N} {YEAR}

📌 Short-term (This Quarter):
• {goal 1}
• {goal 2}
• {goal 3}

🔭 Long-term (This Year):
• {goal 1}
• {goal 2}

📚 Skills to Develop:
• {skill 1}
• {skill 2}
• {skill 3}

💡 To update goals: /goals set
```

If no goals set:
```
🎯 CAREER GOALS

No goals set yet for this quarter.

💡 Set your goals with: /goals set
```

### `/goals set` - Set Goals

Ask questions one at a time:

```
🎯 Let's set your goals for Q{N} {YEAR}

Q1: What are your 2-3 short-term goals for this quarter?
```
{wait for answer}

```
Q2: What are your long-term goals for this year?
```
{wait for answer}

```
Q3: What skills do you want to develop?
```
{wait for answer}

Save to `config/company-context.json` under `career_goals`:

```json
{
  "career_goals": {
    "short_term": ["goal1", "goal2"],
    "long_term": ["goal1"],
    "skills_to_develop": ["skill1", "skill2"],
    "quarter": "Q{N} {YEAR}"
  }
}
```

Confirm:
```
✅ Goals saved for Q{N} {YEAR}!

📌 Short-term:
• {goals...}

🔭 Long-term:
• {goals...}

📚 Skills:
• {skills...}

Good luck achieving your goals! 🚀
```

## Behavior

- View goals: Read config and display, then STOP
- Set goals: Ask questions one at a time, save ONCE, then STOP






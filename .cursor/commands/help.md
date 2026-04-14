# Help Command

Display all available commands and how to use the Quarterly Connection Strategist.

## Instructions

When the user runs this command, display a comprehensive help guide:

```
📚 QUARTERLY CONNECTION COMMANDS

🚀 GETTING STARTED
  /setup             - Configure company values & integrations
  /profile           - Update your name, role, team
  /status            - See current quarter & entry counts

📝 LOGGING
  /log <description> - Add an achievement (auto-categorized)
  /log [Category]: <description>  - Add with specific category
  /logs              - View current quarter's entries
  /logs Q2 2025      - View specific quarter's entries
  /edit              - Edit or delete existing entries

📊 REPORTING
  /preview           - See all data before generating report
  /stats             - Quick metrics (no narrative)
  /report            - Generate full quarterly report
  /report Q3 2025    - Generate report for specific quarter
  /compare Q3 Q4     - Compare two quarters side-by-side

🎯 GOALS
  /goals             - View current career goals
  /goals set         - Set new goals for the quarter

📤 EXPORT
  /export            - Get instructions for PDF export

💡 EXAMPLES
  /log Shipped new auth service reducing latency by 40%
  /log [Challenge]: Fixed production outage in 15 minutes
  /report Q3 2024
  /compare Q2 Q3 2025

📁 LOG CATEGORIES
  • Achievement - Completed milestones, shipped features
  • Challenge - Blockers overcome, problems solved
  • Skill Development - New skills learned, training
  • Collaboration - Cross-team work, mentorship
  • Project Update - Status updates, progress
  • Recognition - Awards, positive feedback

Need more details? Just ask about any command!
```

## Behavior

- Display the help text exactly as shown above
- After displaying, STOP and wait for user input
- Do NOT automatically run any other commands






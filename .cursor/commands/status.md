# Status Command

Show current quarter information and quick counts.

## Instructions

When the user runs `/status`, display:

### Step 1: Calculate Current Quarter

Based on today's date:
- Q1: January 1 - March 31
- Q2: April 1 - June 30
- Q3: July 1 - September 30
- Q4: October 1 - December 31

### Step 2: Check Log File

Read `data/achievements/{YEAR}/Q{N}_log.md` and count entries by category.

### Step 3: Check Configuration

Read `config/company-context.json` to verify setup status.

### Step 4: Display Status

```
📅 QUARTERLY CONNECTION STATUS

Current Quarter: Q{N} {YEAR}
Period: {start_date} - {end_date}
Days Remaining: {days_until_quarter_end}

📝 Log Entries (Q{N}):
├─ Achievements: {count}
├─ Challenges: {count}
├─ Collaborations: {count}
├─ Skill Development: {count}
├─ Project Updates: {count}
├─ Recognition: {count}
└─ Total: {total}

⚙️ Configuration:
├─ Company: {company_name or "Not set"}
├─ GitHub: {username or "Not configured"}
├─ Jira: {email or "Not configured"}
└─ Profile: {name or "Not set"}

💡 Quick Actions:
   /log - Add an achievement
   /report - Generate quarterly report
   /setup - Update configuration
```

## Behavior

- Read files ONCE
- Display status and STOP
- Do NOT automatically fetch GitHub/Jira data
- Do NOT offer to run other commands automatically






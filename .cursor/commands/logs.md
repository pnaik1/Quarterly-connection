# Logs Command

Display log entries for a quarter.

## Instructions

When the user runs `/logs` or `/show logs`, display entries from the log file.

### Parse Input

- `/logs` - Show current quarter
- `/logs Q2` - Show Q2 of current year
- `/logs Q3 2024` - Show Q3 of 2024

### Step 1: Determine Quarter

Calculate which quarter to show based on input or current date.

### Step 2: Read Log File

Read `data/achievements/{YEAR}/Q{N}_log.md`

### Step 3: Display Entries

If file exists with entries:
```
📝 Q{N} {YEAR} LOG ENTRIES

[2025-12-15] | Achievement | Shipped new dashboard feature
[2025-12-14] | Challenge | Fixed critical production bug
[2025-12-12] | Collaboration | Mentored junior engineer on testing
[2025-12-10] | Achievement | Completed API migration project

Total: {count} entries

💡 Commands:
   /log - Add new entry
   /edit - Edit or delete entries
```

If file doesn't exist or is empty:
```
📝 Q{N} {YEAR} LOG ENTRIES

No entries yet for this quarter.

💡 Start logging with:
   /log Shipped new feature with 99% test coverage
```

## Behavior

- Read the log file ONCE
- Display entries and STOP
- Do NOT modify any files
- Do NOT automatically run other commands






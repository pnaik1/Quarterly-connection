# Edit Command

Edit or delete existing log entries.

## Instructions

When the user runs `/edit` or `/edit log`:

### Step 1: Read Current Log

Read `data/achievements/{YEAR}/Q{N}_log.md` for current quarter.

### Step 2: Display Numbered Entries

```
📝 Q{N} {YEAR} LOG ENTRIES

1. [2025-12-15] | Achievement | Shipped new dashboard feature
2. [2025-12-14] | Challenge | Fixed critical production bug
3. [2025-12-12] | Collaboration | Mentored junior engineer
4. [2025-12-10] | Achievement | Completed API migration

Enter the number of the entry to edit/delete (or "cancel"):
```

### Step 3: Handle Selection

Wait for user to enter a number.

If valid number:
```
Selected: [2025-12-14] | Challenge | Fixed critical production bug

What would you like to do?
(e) Edit this entry
(d) Delete this entry
(c) Cancel
```

### Step 4: Process Action

**If Edit (e):**
```
Enter the new text for this entry:
(Keep the same date and category, or include new ones)
```
- Update the entry in the file
- Confirm: "✅ Entry updated!"

**If Delete (d):**
```
Are you sure you want to delete this entry? (yes/no)
```
- If yes, remove the entry from the file
- Confirm: "✅ Entry deleted!"

**If Cancel (c):**
- "Cancelled. No changes made."

## Behavior

- Show numbered list first
- Wait for user selection
- Confirm before deleting
- Write to file ONCE
- After action, STOP and wait for user






# Profile Command

View and update user profile information.

## Instructions

When the user runs `/profile`:

### Step 1: Read Current Profile

Read `config/company-context.json` and extract user_profile section.

### Step 2: Display Current Profile

```
👤 YOUR PROFILE

Name: {name or "Not set"}
Role: {role or "Not set"}
Team: {team or "Not set"}

🔗 Integrations:
GitHub: {github_username or "Not configured"}
Jira: {jira_email or "Not configured"}

What would you like to update?
(name / role / team / all / cancel)
```

### Step 3: Handle Update

Wait for user response:

**If "name":** Ask for new name, update config
**If "role":** Ask for new role, update config
**If "team":** Ask for new team, update config
**If "all":** Ask for name, then role, then team, update config
**If "cancel":** Just stop

### Step 4: Confirm

After updating:
```
✅ Profile updated!

Name: {name}
Role: {role}
Team: {team}
```

## Behavior

- Show current profile first
- Wait for user to choose what to update
- Update config file ONCE
- After confirming, STOP and wait for user






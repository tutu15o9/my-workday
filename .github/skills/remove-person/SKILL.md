---
name: remove-person
description: >
  Remove a person from the people.json contact registry.
  Use when the user wants to delete someone from their contact list.
allowed-tools: Read, Write, Ask
user-invocable: true
---

# Remove Person

Remove a person from `context/people.json`.

## Workflow

1. **Read** the current `context/people.json` file

2. **List** all people currently in the registry (name and email). If the user already named someone, find the match by name, alias, or email.

3. **Confirm** with the user: "Are you sure you want to remove **[Name]** ([email]) from your contact registry? This cannot be undone."

4. If confirmed:
   - **Remove** the person's entry from the `people` array
   - **Write** the updated file back to `context/people.json`
   - **Confirm** the person was removed

5. If not confirmed:
   - Cancel and let the user know nothing was changed

---
name: update-person
description: >
  Update an existing person in the people.json contact registry.
  Use when the user wants to change someone's tone, preferences,
  work context, or other details.
allowed-tools: Read, Write, Ask
user-invocable: true
---

# Update Person

Update an existing person's details in `context/people.json`.

## Workflow

1. **Read** the current `context/people.json` file

2. **List** all people currently in the registry (name and email) so the user can pick one. If the user already named someone, find the match by name, alias, or email.

3. **Show** the person's current entry and ask the user **which fields** to update. Editable fields:
   - Name
   - Aliases
   - Email
   - Relationship (manager, direct-report, peer, stakeholder, skip-level, external, other)
   - Tone
   - Preferred channel (teams, email, meeting)
   - Projects
   - Behavior notes
   - Do not (things to avoid)
   - Work context
   - Timezone
   - Working hours

4. **Collect** the new values for the fields the user wants to change

5. **Show the updated entry** side-by-side with the old one and ask for confirmation

6. **Update** the entry in the `people` array, set `last_updated` to today's date, and write the file back

7. **Confirm** the update was saved successfully

---
name: add-person
description: >
  Add a new person to the people.json contact registry.
  Use when the user wants to add someone with their communication preferences,
  tone, and work context.
allowed-tools: Read, Write, Ask
user-invocable: true
---

# Add Person

Add a new person to `context/people.json` with their communication preferences and work context.

## Workflow

1. **Ask the user** for these details (skip any the user already provided):
   - **Name** (required) — full name
   - **Aliases** — nicknames, display names, short names (optional)
   - **Email** (required) — work email address
   - **Relationship** — one of: manager, direct-report, peer, stakeholder, skip-level, external, other
   - **Tone** — how the user communicates with this person (e.g., "casual and direct", "formal", "friendly but professional")
   - **Preferred channel** — teams, email, or meeting
   - **Projects** — shared workstreams or projects (optional)
   - **Behavior notes** — any useful context (e.g., "prefers bullet points", "responds slowly on Fridays")
   - **Do not** — things to avoid with this person (optional, e.g., "don't mention personal topics")
   - **Work context** — what work the user does with this person
   - **Timezone** — their timezone (optional)
   - **Working hours** — their working hours (optional)

2. **Read** the current `context/people.json` file

3. **Validate** the person doesn't already exist (check by email). If they do, suggest using the `update-person` skill instead.

4. **Build** the new person entry:
   ```json
   {
     "name": "",
     "aliases": [],
     "email": "",
     "relationship": "",
     "tone": "",
     "preferred_channel": "",
     "projects": [],
     "behavior_notes": "",
     "do_not": [],
     "work_context": "",
     "timezone": "",
     "working_hours": "",
     "last_updated": "YYYY-MM-DD"
   }
   ```

5. **Show the entry** to the user and ask for confirmation

6. **Append** the entry to the `people` array in `context/people.json` and write the file back

7. **Confirm** the person was added successfully

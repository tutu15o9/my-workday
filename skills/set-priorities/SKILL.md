---
name: set-priorities
description: >
  View and update your current priorities in context/priorities.json.
  Use when the user wants to set, add, remove, or reorder their priorities,
  weekly goals, blocked items, or deprioritized work.
allowed-tools: Read, Write, Ask
user-invocable: true
---

# Set Priorities

Update the user's priorities in `context/priorities.json`. This file is read by the assistant at the start of every session to rank work and decide what to surface first.

## Workflow

1. **Read** the current `context/priorities.json` file and show it to the user in a readable format

2. **Ask** the user what they want to do:
   - **Replace all** — start fresh with new priorities
   - **Add** — add a new item to a section
   - **Remove** — remove an item from a section
   - **Reorder** — change the ranking of current focus areas
   - **Update** — edit an existing item
   - **Clear a section** — empty out a section (e.g., clear `this_week` for a new week)

3. **Identify the section** to update:
   - **`focus_areas`** — ranked priorities (rank 1 = most important)
   - **`this_week`** — tasks for the current week (with `done` boolean)
   - **`blocked`** — things the user is blocked on (`description` + `waiting_on`)
   - **`deprioritized`** — things that can safely wait (string array)

4. **Collect** the new content from the user

5. **Show the updated JSON** to the user before writing and ask for confirmation

6. **Set `last_updated`** to today's date (YYYY-MM-DD)

7. **Write** the updated file to `context/priorities.json`

8. **Confirm** the update

## File Schema

```json
{
  "last_updated": "YYYY-MM-DD",
  "focus_areas": [
    { "rank": 1, "description": "Highest priority" }
  ],
  "this_week": [
    { "task": "Task description", "done": false }
  ],
  "blocked": [
    { "description": "What's blocked", "waiting_on": "person or team" }
  ],
  "deprioritized": [
    "Things that can wait"
  ]
}
```

## Rules

- Always renumber `rank` in `focus_areas` sequentially after any change
- When marking a `this_week` task complete, set `done` to `true`
- Never remove a section key — if empty, use an empty array `[]`
- Always update `last_updated` to today's date when making changes

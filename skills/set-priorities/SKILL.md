---
name: set-priorities
description: >
  View and update your current priorities in context/priorities.md.
  Use when the user wants to set, add, remove, or reorder their priorities,
  weekly goals, blocked items, or deprioritized work.
allowed-tools: Read, Write, Ask
user-invocable: true
---

# Set Priorities

Update the user's priorities in `context/priorities.md`. This file is read by the assistant at the start of every session to rank work and decide what to surface first.

## Workflow

1. **Read** the current `context/priorities.md` file and show it to the user

2. **Ask** the user what they want to do:
   - **Replace all** — start fresh with new priorities
   - **Add** — add a new item to a section
   - **Remove** — remove an item from a section
   - **Reorder** — change the ranking of current focus areas
   - **Update** — edit an existing item
   - **Clear a section** — empty out a section (e.g., clear "This Week" for a new week)

3. **Identify the section** to update:
   - **Current Focus Areas** — numbered, ranked priorities (most important first)
   - **This Week** — checkbox tasks for the current week
   - **Blocked / Waiting On** — things the user is blocked on and who can unblock
   - **Deprioritized** — things that can safely wait (helps the assistant not surface these)

4. **Collect** the new content from the user. For each item, get:
   - The priority text (what it is)
   - For focus areas: the rank / order
   - For blocked items: what's blocking and who can unblock
   - For weekly tasks: whether it's done or not

5. **Show the updated priorities.md** to the user before writing. Display a clean diff or before/after so they can confirm.

6. **Write** the updated file to `context/priorities.md`

7. **Confirm** the update and remind the user that the assistant will use these priorities to rank their work in future sessions.

## File Format

Always maintain this structure in `context/priorities.md`:

```markdown
# My Priorities

## Current Focus Areas
1. [Highest priority]
2. [Second priority]
3. [Third priority]

## This Week
- [ ] [Incomplete task]
- [x] [Completed task]

## Blocked / Waiting On
- [Description of blocker] — waiting on [person/team]

## Deprioritized (OK to defer)
- [Things that can wait]
```

## Rules

- Always keep items numbered in "Current Focus Areas" (they represent rank)
- Use checkboxes (`- [ ]` / `- [x]`) in "This Week"
- Never delete a section — if it's empty, leave the heading with no items
- Set today's date in a comment at the top when updating: `> Last updated: YYYY-MM-DD`

---
name: manage-todo
description: >
  Manage the interactive todo board. Generate a new todo from the daily briefing,
  archive old todos, add/remove items, or mark items done. The todo file lives at
  context/todo.json and archives go to context/archive/.
allowed-tools: Read, Write, Ask
user-invocable: true
---

# Manage Todo

Manage the user's interactive todo board at `context/todo.json`.

## Actions

### Generate a new todo list

When the user asks to generate a todo (e.g., "make me a todo", "start my day", after a briefing):

1. **Check** if `context/todo.json` already has items. If yes, **archive first** (see Archive below).
2. **Gather** items from the daily briefing or user input. Group into categories:
   - **Meetings** — today's meetings that need action or prep (source: calendar)
   - **Email** — emails needing a reply or action (source: mail)
   - **Teams** — unread messages needing a response (source: teams)
   - **Engineering** — ADO work items, PR reviews, code tasks (source: ado)
   - **Follow-ups** — items from `context/followups.json` (source: followups)
   - Add other categories as appropriate. Omit empty categories.
3. **Apply ignore filters** from `context/me.json` `communication_preferences.ignore` — do not include filtered items.
4. **Build** the todo JSON:
   ```json
   {
     "created": "YYYY-MM-DD",
     "source": "daily-briefing",
     "categories": [
       {
         "name": "Category Name",
         "items": [
           {
             "id": "short-kebab-id",
             "text": "Description of the task",
             "done": false,
             "source": "calendar|mail|teams|ado|followups|manual"
           }
         ]
       }
     ]
   }
   ```
5. **Show** the todo list to the user and ask for confirmation.
6. **Write** to `context/todo.json`.

### Archive the current todo

When generating a new todo or when the user asks to archive:

1. **Read** `context/todo.json`.
2. **Copy** the file to `context/archive/todo-{created-date}.json` with an added `"archived": "YYYY-MM-DD"` field. If a file with that name already exists, append a counter (e.g., `todo-2026-05-19-2.json`).
3. **Extract undone items** — any item with `"done": false` carries forward.
4. **Write** the new todo to `context/todo.json` with carried-over undone items plus any new items.

### Add an item

1. **Ask** for: text, category (suggest existing ones), and source (optional).
2. **Read** `context/todo.json`.
3. **Add** the item to the appropriate category (create the category if it doesn't exist).
4. **Generate** an ID from the text (kebab-case, 3-4 words).
5. **Write** the updated file.

### Remove an item

1. **Read** `context/todo.json`.
2. **List** all items with IDs. If the user already named one, find the match.
3. **Confirm** removal.
4. **Remove** the item. If the category is now empty, remove the category too.
5. **Write** the updated file.

### Mark done / undone

1. **Read** `context/todo.json`.
2. **Find** the item by ID or text match.
3. **Toggle** the `done` field.
4. **Write** the updated file.
5. **Confirm** the change.

### View archive

When the user asks about past todos:

1. **List** files in `context/archive/` sorted by date.
2. **Show** a summary (date, completion percentage, number of items).
3. If the user wants details, read the specific archive file.

## Rules

- Always apply ignore filters when generating todos.
- Always archive before generating a new todo if the current one has items.
- Carry forward undone items when archiving.
- Use kebab-case IDs (e.g., `reply-avijeet-meeting`, `fix-playwright-pipeline`).
- Keep item text concise — one line, actionable.
- When carrying forward undone items, deduplicate by `id` — if the new briefing produces an item with the same ID, keep the carried-forward version.
- The HTML board at `todo.html` reads directly from `context/todo.json` — no need to update the HTML.

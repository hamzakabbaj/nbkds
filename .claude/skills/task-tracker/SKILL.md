---
name: task-tracker
description: Read and write the NOOBA "Suivi des tâches" task tracker in Notion. Use when the user asks to list open tasks, check what's in progress, update a task's status, create a task, or file a bug/feature from the repo.
allowed-tools: mcp__claude_ai_Notion__notion-fetch, mcp__claude_ai_Notion__notion-search, mcp__claude_ai_Notion__notion-create-pages, mcp__claude_ai_Notion__notion-update-page, mcp__claude_ai_Notion__notion-get-users
---

You manage the NOOBA project's **"Suivi des tâches"** (task tracker), a Notion database. This skill pins down the IDs, the exact property values, and the read/write conventions so you don't have to re-derive the schema each session.

## Identifiers

- **Database**: `36360a16-e54f-80fe-93f2-f527092d85db`
- **Data source** (use this for queries/creates): `collection://36360a16-e54f-8050-8649-000b9949c4e1`
- **Default page template**: `36360a16-e54f-8066-ba7d-fabfe3c731d6`
- URL: https://www.notion.so/36360a16e54f80fe93f2f527092d85db

If a tool ever reports these IDs are stale, re-discover with `notion-search` (query "Suivi des tâches") and update this file.

## Schema (exact values — copy verbatim, they're French with accents)

| Property | Type | Allowed values |
|---|---|---|
| **Nom de la tâche** | title | free text (required) |
| **Description** | text | free text |
| **État** | status | `Backlog`, `Sprint Backlog`, `En cours`, `Bloqué`, `A valider`, `Terminé` |
| **Priorité** | select | `Élevée`, `Moyenne`, `Faible` |
| **Niveau d'effort** | select | `Faible`, `Moyenne`, `Élevé` |
| **Type de tâche** | multi-select | `🐞 Bug`, `💬 Demande de fonctionnalité`, `💅 Perfectionnement` |
| **Personne assignée** | person | user IDs (use `notion-get-users` / `notion-search` query_type:user to resolve) |
| **Date d'échéance** | date | ISO-8601 |
| **Ordre** | number | board sort order (lower = higher on the board) |

**État status groups** (for "is this open/done?"):
- To-do: `Backlog`, `Sprint Backlog`
- In progress: `En cours`, `Bloqué`, `A valider`
- Complete: `Terminé`

## Views (for reference)

- **Toutes les tâches** — board grouped by État, sorted by Ordre
- **Par état** — board grouped by État (shows assignee, priority, type)
- **Mes tâches** — table filtered to the current user
- **Liste de contrôle** — simple list (État + name)

## Reading tasks

Use `notion-search` with `data_source_url: "collection://36360a16-e54f-8050-8649-000b9949c4e1"` and a semantic query, OR `notion-fetch` on the data source to inspect rows. To answer "what's open / in progress / blocked", filter by **État** using the groups above. Report tasks grouped by État, ordered by **Ordre**.

## Creating a task

Use `notion-create-pages` with `parent: { data_source_url: "collection://36360a16-e54f-8050-8649-000b9949c4e1" }`. Set properties using the exact values from the schema table. Minimum useful task = **Nom de la tâche** + **État** + **Priorité** + **Type de tâche**.

Conventions:
- New work normally starts in **`Backlog`** (or **`Sprint Backlog`** if the user says it's for the current sprint).
- A bug filed from the repo → **Type de tâche** = `🐞 Bug`, put repro steps / file references in **Description** (use `file_path:line` references).
- Set **Ordre** only if the user cares about board position; otherwise leave it.

## Updating a task

Use `notion-update-page` on the task's page ID. Common updates:
- Move status: set **État** to the next value (e.g. `En cours` → `A valider` → `Terminé`).
- Reassign: set **Personne assignée** (resolve the person first).

Never write to read-only/system-managed properties (createdTime, etc.).

### Date field gotcha

**Date d'échéance** is a date column. When creating/updating via the SQLite-style interface, set the **expanded** fields, not the bare column:
- `date:Date d'échéance:start` — ISO-8601 date/datetime (required)
- `date:Date d'échéance:end` — only for ranges; NULL for a single date
- `date:Date d'échéance:is_datetime` — `1` for datetime, `0`/NULL for date-only

## Etiquette

Writing to Notion is an outward-facing action on a shared workspace. Before creating or mutating tasks **in bulk** or changing someone else's assignment, confirm with the user. A single task create/update the user explicitly asked for needs no extra confirmation. After writing, report back the task name + new État (and the Notion URL).

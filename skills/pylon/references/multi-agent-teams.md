# Multi-Agent Teams

When a master agent spawns subagents (e.g. via Claude Code teams), use projects to organize all documents under one umbrella.

## Setup (master agent)

```
1. Master pushes first plan:
   push_plan(plan="...", source="master", group="auth-refactor", context="...")
   → Returns document_id, project_id
   → Session group locked to "auth-refactor"

2. Master sets project description:
   update_project(project="auth-refactor", description="Refactoring auth to JWT. Frontend: login UI. Backend: token validation.")

3. Master notes the document_id and creates tasks for subagents.
```

## Subagent Pushes

Each subagent pushes its own plan — creates a NEW document under the same project:

```
backend-dev:
  push_plan(plan="...", source="backend-dev", group="auth-refactor", context="...")
  → New document, joins "auth-refactor" project
  → Save document_id for re-pushes

frontend-dev:
  push_plan(plan="...", source="frontend-dev", group="auth-refactor", context="...")
  → Another new document under "auth-refactor"
  → Save document_id for re-pushes
```

## Rules

- **Each agent gets its own document.** Never share a `document_id` across agents.
- **All agents use the same `group`.** Groups everything under one project in the web UI.
- **Session group is sticky.** First value wins — subagents cannot override.
- **Re-pushes need explicit `document_id`.** Without it, you create a duplicate.
- **Pull from your own doc.** `pull_final_plan(document_id="<my-doc-id>")`
- **Always set `source`.** Lets the human reviewer identify each agent.
- **Always set `context`.** Give the reviewer enough background for each agent's work.

## Same-Session Safety

When multiple agents share one MCP session:

- **Always pass `document_id` explicitly** — the session's "current document" is shared state.
- `push_plan` without `document_id` always creates a new document (safety feature).
- Use `create_new: true` when starting a different topic.
- Save `document_id` from the first push response and reuse it.

## Checking Project Status

```
list_documents(group="auth-refactor")
→ Returns all documents with status, locks, sources
```

The human sees all documents grouped under the project name, each showing its source agent and context.

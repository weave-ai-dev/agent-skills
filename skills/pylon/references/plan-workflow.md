# Plan Workflow

## Starting a New Task (default)

Every new user request should start with a fresh push — **never reuse document IDs from previous tasks or conversations**.

```
1. Agent generates a plan
2. push_plan(plan="...", source="claude-code", context="User wants to...", group="my-project")
   → Do NOT pass document_id — the server creates a new document automatically
   → Returns document_id, URL, project_id
3. update_project(project="my-project", description="Brief description for reviewers")
4. Human reviews at the returned URL
5. Human edits, comments, snapshots
6. pull_plan()
   → Gets reviewed content with plain_text, decisions, terminal_context
7. Agent proceeds with the approved plan
```

## Re-push (revising based on feedback)

Only pass `document_id` when updating the **same document within the same task** after receiving human feedback:

```
1. Agent revises plan based on pull_plan feedback
2. push_plan(document_id="<id-from-original-push>", plan="...", context="Updated based on feedback...")
   → Overwrites content, creates next version (v2, v3, ...)
3. Human sees live-refresh banner and reviews the new version
```

## New Topic (different plan entirely)

When switching to a completely different topic mid-session:

```
1. push_plan(plan="...", create_new=true, context="...")
   → Creates a brand new document + v1
   → Session switches to the new document
```

## Discovery Flow

If the user doesn't know their document ID:

```
1. list_documents() or list_documents(group="my-project")
   → Returns list of documents with IDs, titles, status
2. User picks a document
3. use_document(document_id="<id>")
   → Sets session context
4. pull_plan()
   → Gets the content
```

## Versioning

- Every `push_plan` creates a new version automatically
- Every `pull_plan` (without specifying a version) auto-snapshots before returning
- `list_versions()` shows all version history
- `pull_plan(version=3)` retrieves a specific past version

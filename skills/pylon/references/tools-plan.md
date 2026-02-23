# Plan Tools

## push_plan

Push a proposed plan for human review. Every push auto-creates a version snapshot.

| Parameter | Required | Description |
|---|---|---|
| `plan` | Yes | Markdown content of the plan |
| `source` | No | Agent identifier, e.g. "claude-code", "backend-dev". **Always set this.** |
| `context` | No | Brief summary of your conversation — key decisions, requirements, background. |
| `document_id` | No | Target document to update. Omit to create a new document. |
| `create_new` | No | Set `true` to force-create a new document even if `document_id` is provided. |
| `group` | No | Project name (e.g. "auth-refactor"). **Sticky per session** — first push locks it. Auto-creates project. |
| `force_unlock` | No | Clear a stale session lock before acquiring. Does NOT bypass web editing guard. |

**Returns:** `document_id`, `title`, `url`, `version`, `project`, `project_id`, `action` ("created" or "updated")

## pull_plan

Pull the latest content from a document after human review. **Auto-snapshots** the current state as a new version.

| Parameter | Required | Description |
|---|---|---|
| `document_id` | No | If omitted, uses session's current document. |
| `version` | No | Version number or `"latest"` for a specific past version. |
| `force_unlock` | No | Clear a stale lock. Only applies to live doc pulls (no version). |

**Returns:**
- `title`, `content`, `updated_at` — the document data
- `plain_text` — plain text extracted from TipTap JSON (no markup)
- `decisions` — array of resolved threads: `{ thread_id, anchor_text, status, messages: [{ author_type, content }] }`
- `terminal_context` — the conversation context set during push
- `version` — version number of the auto-created snapshot
- `web_editing_active` — (boolean) present if human had unsnaphotted web edits

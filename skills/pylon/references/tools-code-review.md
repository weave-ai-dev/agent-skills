# Code Review Tools

## push_code_review

Push file diffs for human code review. Creates a `code_review` document with full file contents for side-by-side diff viewing.

| Parameter | Required | Description |
|---|---|---|
| `files` | Yes | Array of file changes (see format below) |
| `code_review_id` | No | Existing code review to update. Omit to create new. |
| `plan_document_id` | No | Link to the parent plan document. |
| `title` | No | Title for the code review. Auto-generated from file paths if omitted. |
| `summary` | No | Brief summary of what the changes do. |
| `context` | No | Terminal conversation context for the web UI. |
| `source` | No | Agent identifier (e.g. "backend-dev"). |
| `group` | No | Project name. Sticky per session. |
| `force_unlock` | No | Clear a stale session lock. |

### File Change Format

```json
{
  "path": "src/auth/login.ts",
  "status": "modified",
  "old_content": "// original file contents...",
  "new_content": "// updated file contents...",
  "old_path": "src/old-path.ts",
  "language": "typescript"
}
```

- `status`: `"added"` | `"modified"` | `"deleted"` | `"renamed"`
- `old_content`: Original file (empty string for new files)
- `new_content`: Updated file (empty string for deleted files)
- `old_path`: Only needed for `"renamed"` status
- Full file contents required — Monaco needs both sides for diff

**Returns:** `code_review_id`, `url`, `version`, `files_count`, `plan_document_id`, `project`, `project_id`

## pull_code_feedback

Pull human feedback from a code review. Returns threads grouped by file path.

| Parameter | Required | Description |
|---|---|---|
| `code_review_id` | No | Falls back to session's current code review. |
| `include_resolved` | No | Include resolved threads. Default: false. |

**Returns:**
- `review_status` — "open", "approved", "changes_requested", "rejected"
- `feedback` — object keyed by file path, each with: `thread_id`, `line_start`, `line_end`, `anchor_text`, `resolved`, `messages[]`
- `unresolved_count` — number of open threads
- `plan_document_id` — linked plan if any

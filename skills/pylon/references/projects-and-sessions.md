# Projects & Sessions

## Projects

Projects are first-class entities that group related documents. Created via the `group` param on `push_plan` or `push_code_review`.

- **Auto-creation**: First push with `group: "auth-refactor"` creates a project named "auth-refactor".
- **Session-sticky**: Once set, all subsequent pushes in the session inherit the group — even without passing `group` again.
- **Cannot be overridden**: If the session group is "auth-refactor", passing `group: "different"` still uses "auth-refactor". Prevents accidental fragmentation.
- **Description**: `update_project(project: "auth-refactor", description: "Refactoring auth to JWT...")` gives human reviewers context.
- **Filtering**: `list_documents(group: "auth-refactor")` returns only documents in that project.

## Session Locking

Each MCP session holds an advisory lock on documents it operates on. Prevents multiple terminal sessions from conflicting on the same document.

- **Automatic**: `push_plan`, `pull_plan`, `use_document` auto-acquire locks.
- **Exclusive**: If another session holds the lock → `document_locked` error with expiry info.
- **TTL**: Locks expire after 30 minutes of inactivity. No heartbeat needed.
- **Force unlock**: `force_unlock: true` clears a stale lock from a dead session. Only clears session locks — does NOT bypass web editing guard.
- **Explicit release**: `release_document()` unlocks before TTL expires.
- **Session cleanup**: All locks released on disconnect.
- **Web UI unaffected**: Locks only apply to MCP operations; the web editor works normally.
- **Discovery**: `list_documents` shows `locked` and `locked_by_me` flags.

## Web Editing Guard

When a human is actively editing in the web UI, terminal pushes are blocked to prevent overwriting their work. **One-directional** — the human is never locked out.

- **How it works**: Web auto-save sets a `web_edited_at` timestamp. If a terminal `push_plan` arrives within 5 minutes, it's rejected with `web_editing_active`.
- **Error response**: `{ error: "web_editing_active", message: "...", web_edited_at: "...", retry_after_ms: <remaining> }`
- **Clearing**: Human clicks "Snapshot" in the web UI, or terminal calls `pull_plan`. Both clear the flag.
- **TTL expiry**: If the human stops editing for 5+ minutes, the guard expires.
- **force_unlock does NOT bypass this**: Only clears stale session locks.

**When you get `web_editing_active`**: Tell the user a human is reviewing. Wait for snapshot or TTL expiry, then retry.

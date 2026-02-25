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

## MCP Connection Recovery

The Pylon MCP server uses in-memory sessions. Sessions are lost when:
- The server restarts or redeploys
- The conversation is cleared (`/clear`) or compacted (`/compact`)
- The session TTL (1 hour) expires

**Stale session auto-recovery:**

The server handles expired sessions gracefully — no HTTP 404 errors that break the MCP client:

- **`initialize` requests** with a stale session ID automatically create a new session. The client picks up the new session ID from the response header.
- **Tool calls** (e.g. `push_plan`, `pull_plan`) with a stale session ID receive a JSON-RPC error (code -32001, "Session expired") instead of HTTP 404. This allows the MCP client to surface the error normally and re-initialize.

**When you get a session-expired error:**

1. **Do NOT panic or tell the user the server is down.** This is normal after `/clear`, `/compact`, or a deploy.
2. **The MCP client should automatically re-initialize** a new session. If it doesn't, ask the user to run `/mcp` to restart the MCP connection.
3. **You lose no data.** Documents, projects, versions, and comments are all persisted in the database. Only the ephemeral session state (current document, session group, session locks) is lost.
4. **After reconnection, resume by discovering existing work:**
   - Call `list_documents()` to see all your documents and projects.
   - Call `list_documents(group="project-name")` to find documents in a specific project.
   - Call `use_document(document_id="...")` to set context on an existing document.
   - Call `pull_plan(document_id="...")` to get the latest content and any human feedback.
   - Pass `group` again on the next `push_plan` to re-associate with the project (session group resets on reconnect).
   - For a brand new task: just call `push_plan` without `document_id` as usual.

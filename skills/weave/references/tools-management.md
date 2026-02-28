# Management Tools

## list_documents

List documents owned by the current user, optionally filtered.

| Parameter | Required | Description |
|---|---|---|
| `title_contains` | No | Case-insensitive title filter. |
| `group` | No | Filter by project name. |

**Returns:** Array of documents (up to 50) with `id`, `title`, `updated_at`, `locked`, `locked_by_me`, `project`, `project_id`, `source`, `project_description`, `terminal_context`.

## list_versions

List all version snapshots for a document.

| Parameter | Required | Description |
|---|---|---|
| `document_id` | No | If omitted, uses session's current document. |

**Returns:** Array of `{ version_number, source, created_at, preview }` (preview = first 120 chars of plain text).

## use_document

Switch the session's current document context.

| Parameter | Required | Description |
|---|---|---|
| `document_id` | Yes | The document to switch to. |

**Returns:** Document `title`, `updated_at`.

## update_project

Update a project's metadata. Projects group related documents.

| Parameter | Required | Description |
|---|---|---|
| `project` | Yes | Current project name. |
| `description` | No | New description displayed on the project header. |
| `rename` | No | Rename the project. Updates all documents. |
| `archive` | No | Set `true` to archive, `false` to unarchive. Archived projects block all push/pull operations. |

**Returns:** `project` (new name), `project_id`, `description`, `renamed_from` (if renamed), `archived` (if changed).

## release_document

Release the session lock on a document so other sessions can operate on it.

| Parameter | Required | Description |
|---|---|---|
| `document_id` | No | If omitted, releases the session's current document. |

**Returns:** `status` ("ok" or "not_locked_by_you"), `document_id`.

---
name: pylon
description: Pylon MCP tools for pushing plans, code reviews, and pulling human feedback. Use this skill when interacting with the Pylon AI-to-human review bridge.
license: MIT
metadata:
  author: weave-ai-dev
  version: "1.0.0"
  organization: Weave AI
  date: February 2026
  abstract: Reference guide for Pylon's MCP tools. Covers push_plan, pull_plan, push_code_review, pull_code_feedback, list_documents, list_versions, use_document, update_project, and release_document.
---

# Pylon

Pylon is an AI-to-human review bridge. AI agents push proposed plans and code diffs via MCP; humans review, comment, and approve via a rich web editor.

## MCP Tools

| Tool | Purpose |
|---|---|
| `push_plan` | Push a plan for human review. Auto-creates doc + project + version. |
| `pull_plan` | Pull reviewed content with decisions and feedback. |
| `push_code_review` | Push file diffs for side-by-side code review. |
| `pull_code_feedback` | Pull line-specific review comments grouped by file. |
| `list_documents` | List documents, filter by title or project. |
| `list_versions` | List version snapshots for a document. |
| `use_document` | Switch session's current document context. |
| `update_project` | Update project description or rename. |
| `release_document` | Release session lock on a document. |

See `references/tools-plan.md`, `references/tools-code-review.md`, and `references/tools-management.md` for full parameter docs.

## Setup

Add Pylon's MCP server to your agent config. See `references/setup.md` for per-agent instructions.

**Claude Code** (`.mcp.json`):
```json
{
  "mcpServers": {
    "pylon": {
      "type": "streamable-http",
      "url": "https://your-pylon-instance.com/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

Generate a token at **Settings > Auth Tokens** in the Pylon web UI.

## Key Principles

1. **Native MCP calls only.** Never write scripts or temp files to interact with Pylon.
2. **Always set `source`.** Identify yourself (e.g. "claude-code", "backend-dev") so the human knows which agent produced each document.
3. **Always set `context`.** Summarize your conversation — helps the web UI's AI make better suggestions.
4. **Document auto-creation.** `push_plan` without `document_id` creates a new document. Pass `document_id` explicitly to update.
5. **Session memory.** After a push, the session remembers the current document for subsequent pulls.

## Quick Workflow

```
1. push_plan(plan="...", source="claude-code", group="my-project", context="...")
   → Creates document + project, returns document_id + URL
2. Human reviews, comments, edits at the URL
3. pull_plan()
   → Returns reviewed content, decisions, plain_text
4. Proceed with the approved plan
```

See `references/plan-workflow.md` for re-push, versioning, and advanced patterns.
See `references/code-review-workflow.md` for the code review lifecycle.
See `references/multi-agent-teams.md` for team/swarm coordination.

## Core Concepts

- **Projects** group related documents via the `group` param. Auto-created, session-sticky. See `references/projects-and-sessions.md`.
- **Session locking** prevents terminal session conflicts (30-min TTL). See `references/projects-and-sessions.md`.
- **Web editing guard** blocks terminal pushes while a human is editing (5-min TTL). Human is never locked out.
- **Code reviews** use side-by-side Monaco diffs with line-specific threaded comments. See `references/code-review-workflow.md`.

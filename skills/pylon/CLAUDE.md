# Pylon MCP Skill

## Structure

```
pylon/
  SKILL.md       # Main skill file - read this first
  AGENTS.md      # This navigation guide
  CLAUDE.md      # Copy of AGENTS.md
```

## Usage

1. Read `SKILL.md` for the full MCP tool reference
2. The Pylon MCP tools are available natively when the dev server is running and `.mcp.json` is configured

Pylon is an AI-to-Web review bridge. AI agents push proposed plans via MCP; humans review, comment, and approve via the web UI.

**Golden Rule:** New user request = new project. Always omit `document_id` on the first push.

## MCP Tools Summary

| Tool | Purpose |
|---|---|
| `push_plan` | Push a plan for human review (auto-creates doc + project) |
| `pull_plan` | Pull reviewed markdown, decisions, and open feedback |
| `push_code_review` | Push file diffs for human code review (side-by-side Monaco diff) |
| `pull_code_feedback` | Pull line-specific feedback from a code review |
| `list_documents` | List user's documents, filter by title or project |
| `list_versions` | List version snapshots for a document |
| `use_document` | Switch session's current document context |
| `update_project` | Update project description or rename |
| `release_document` | Release session lock on a document |

## Key Concepts

- **Projects**: Group related documents via `group` param on `push_plan`. Auto-created, session-sticky.
- **Code Reviews**: Agents push file diffs via `push_code_review`; humans review in read-only Monaco diff viewer with line-specific comments.
- **Session locking**: Advisory locks prevent terminal session conflicts. 30-min TTL.
- **Web editing guard**: Terminal pushes blocked while human is actively editing (5-min TTL). Human is never locked out.
- **Multi-agent teams**: Master sets `group`, all subagents push to same project. Each agent gets its own document/code review.

## Slash Commands

| Command | Description |
|---|---|
| `/project:pylon-push` | Push a plan to Pylon |
| `/project:pylon-pull` | Pull reviewed content |
| `/project:pylon-list` | List your documents |

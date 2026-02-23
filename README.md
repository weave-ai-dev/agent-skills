# Weave Agent Skills

Official [Agent Skills](https://agentskills.io) for [Pylon](https://github.com/weave-ai-dev/pylon) — the AI-to-human review bridge.

## Install

```bash
npx skills add weave-ai-dev/agent-skills
```

This installs the Pylon skill into your AI agent (Claude Code, Cursor, Windsurf, Copilot, Gemini CLI, and [30+ others](https://agentskills.io)).

### Install for a specific agent

```bash
npx skills add weave-ai-dev/agent-skills -a claude-code
```

### Install globally

```bash
npx skills add weave-ai-dev/agent-skills -g
```

## What's included

### `pylon`

MCP tools for pushing plans, code reviews, and pulling human feedback. Teaches your AI agent how to use Pylon's review workflow.

| Tool | Purpose |
|---|---|
| `push_plan` | Push a plan for human review |
| `pull_final_plan` | Pull reviewed content with decisions |
| `push_code_review` | Push file diffs for code review |
| `pull_code_feedback` | Pull line-specific review comments |
| `list_documents` | List and filter documents |
| `list_versions` | List version snapshots |
| `use_document` | Switch document context |
| `update_project` | Update project metadata |
| `release_document` | Release session lock |

## Prerequisites

1. A running Pylon instance
2. An MCP auth token (generate at **Settings > Auth Tokens**)
3. MCP server configured in your agent (see `skills/pylon/references/setup.md`)

## License

MIT

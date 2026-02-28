# Multi-Agent Teams

When a lead agent spawns subagents (e.g. via Claude Code teams), coordinate through the lead agent for all Weave MCP calls.

## Important: MCP Access

**Subagents (teammates) do NOT have access to MCP tools.** Only the lead agent can call Weave tools like `push_plan`, `pull_plan`, `push_code_review`, and `pull_code_feedback`.

Subagents should:
1. Do their research, planning, or coding work
2. **Send the result back to the lead agent via `SendMessage`**
3. The lead agent pushes to Weave on their behalf

Never write scripts, curl commands, or workarounds to call MCP from subagents.

## Workflow

### Lead agent setup

```
1. Create team and assign tasks to subagents
2. Subagents work on their tasks and send results back
3. Lead agent collects results and pushes to Weave:
   push_plan(plan="...", source="team-lead", group="auth-refactor", context="...")
   → Returns document_id + URL
4. Share URLs with user and STOP IMMEDIATELY
   → Do NOT say "standing by", "agents are idle", or "let me know when ready"
   → Do NOT send follow-up messages — one message with the URLs is enough
   → Shut down idle subagents to stop burning tokens
```

### Multiple documents (one per subagent's work)

The lead agent can push separate documents for different subagents' work:

```
lead pushes backend plan:
  push_plan(plan="<backend-agent's plan>", source="backend-dev", group="auth-refactor", context="...")
  → New document under "auth-refactor"

lead pushes frontend plan:
  push_plan(plan="<frontend-agent's plan>", source="frontend-dev", group="auth-refactor", context="...")
  → Another new document under "auth-refactor"
```

### Pulling feedback

```
1. User reviews on Weave and asks the lead to pull
2. Lead pulls:
   pull_plan(document_id="<backend-doc-id>")
   pull_plan(document_id="<frontend-doc-id>")
3. Lead sends relevant feedback to each subagent via SendMessage
4. Subagents address feedback and send updated plans back
5. Lead re-pushes with document_id to update the same documents
```

## Rules

- **Lead agent owns all MCP calls.** Subagents communicate via SendMessage.
- **Each piece of work gets its own document.** Never share a `document_id` across concerns.
- **All documents use the same `group`.** Groups everything under one project in the web UI.
- **Re-pushes need explicit `document_id`.** Without it, you create a duplicate.
- **Always set `source`.** Use the subagent's name/role so the human reviewer can identify each agent's work.
- **Always set `context`.** Give the reviewer enough background for each agent's work.

## Checking Project Status

```
list_documents(group="auth-refactor")
→ Returns all documents with status, locks, sources
```

The human sees all documents grouped under the project name, each showing its source agent and context.

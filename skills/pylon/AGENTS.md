# Pylon MCP Skill

## Structure

```
pylon/
├── SKILL.md                            # Main skill — start here
├── AGENTS.md                           # This navigation guide
├── CLAUDE.md                           # Copy of AGENTS.md
└── references/
    ├── setup.md                        # MCP setup for Claude Code, Cursor, etc.
    ├── tools-plan.md                   # push_plan, pull_final_plan parameters
    ├── tools-code-review.md            # push_code_review, pull_code_feedback parameters
    ├── tools-management.md             # list_documents, list_versions, use_document, etc.
    ├── plan-workflow.md                # Plan push/pull lifecycle and patterns
    ├── code-review-workflow.md         # Code review lifecycle and statuses
    ├── projects-and-sessions.md        # Projects, session locking, web editing guard
    └── multi-agent-teams.md            # Team/swarm coordination patterns
```

## Quick Reference

| I want to... | Read |
|---|---|
| Set up Pylon MCP connection | `references/setup.md` |
| Push a plan for review | `SKILL.md` + `references/tools-plan.md` |
| Push code diffs for review | `references/tools-code-review.md` |
| List/manage documents | `references/tools-management.md` |
| Understand the plan review loop | `references/plan-workflow.md` |
| Understand code review loop | `references/code-review-workflow.md` |
| Work with projects & locks | `references/projects-and-sessions.md` |
| Coordinate a multi-agent team | `references/multi-agent-teams.md` |

# Weave

**Stop pushing AI hallucinations to your repo.** Review AI-generated plans and code *before* they hit GitHub.

```bash
npx skills add weave-ai-dev/agent-skills
```

---

## The Problem

If you use Claude Code, Cursor, or any terminal-based AI agent, you already know these two pain points:

**1. You can't review AI plans.** Your agent generates a 300-line architecture plan in the CLI. You can't open a thread on line 42. You can't drill into a specific decision. Your only option is to copy-paste snippets back into the terminal and argue. It's slow, lossy, and painful.

**2. Your commits are dirty.** When the AI writes code, you push it to GitHub just to see the diff. That means AI hallucinations, untested assumptions, and garbage code are now polluting your PR history — visible to your entire team. There's no private review step.

Weave fixes both.

---

## What It Does

Weave is a **human-agent collaboration bridge**. It gives you a Web HUD where you can review and refine your AI agent's work before it becomes real code.

The loop:

```
1. Plan   → Your AI pushes an architecture plan to Weave
2. Review → You open the web UI, do inline edits, start threaded discussions
3. Approve → Hit approve, your AI pulls the finalized plan and writes the code
4. Review → The AI pushes the code diff back to Weave for private, line-by-line review
5. Ship   → Only clean, human-reviewed code hits your repo
```

Think of it as a **private staging layer** between your AI agent and your Git history.

---

## Install in 30 Seconds

Works with Claude Code, Cursor, Windsurf, Copilot, Gemini CLI, and 30+ agents.

```bash
# Auto-detect your agent
npx skills add weave-ai-dev/agent-skills

# Or specify your agent
npx skills add weave-ai-dev/agent-skills -a claude-code

# Global install (available across all projects)
npx skills add weave-ai-dev/agent-skills -g
```

**Prerequisites:**
1. A Weave account → [Sign up here](https://useweave.dev)
2. That's it. OAuth handles authentication automatically — your tool opens a browser to sign in on first connect.

---

## What Your Agent Gets

Once installed, your AI agent has access to these MCP tools:

| Tool | What it does |
|---|---|
| `push_plan` | Submit an architecture plan for your review |
| `pull_plan` | Pull back your reviewed decisions and edits |
| `push_code_review` | Submit file diffs for private code review |
| `pull_code_feedback` | Retrieve your line-by-line comments |
| `list_documents` | Browse and filter project documents |
| `list_versions` | Access version history snapshots |
| `use_document` | Switch document context |
| `update_project` | Update project metadata |
| `release_document` | Release session locks |

---

## How It Looks in Practice

**In your terminal:**
```
You: Build me a REST API for user authentication with JWT

Agent: I've pushed an architecture plan to Weave for your review.
       → View it at: https://app.useweave.dev/projects/abc123

       Waiting for your approval...
```

**In the Weave Web HUD:**
- Read the full plan with syntax highlighting
- Click any line to start a threaded discussion
- Make inline edits directly
- Hit "Approve" when you're satisfied

**Back in your terminal:**
```
Agent: Plan approved. Writing code now...
       ...
       Code complete. I've pushed the diff to Weave for your review.
       → Review at: https://app.useweave.dev/projects/abc123/review
```

---

## Why This Matters

- **No more dirty commits.** Review AI code privately before it touches your repo.
- **No more copy-paste arguments.** Discuss specific lines in threaded conversations, not terminal back-and-forth.
- **No more blind trust.** You see exactly what the AI intends to build before a single line of code is written.
- **No workflow disruption.** One `npx` command. Your agent handles the rest. You stay in your flow.

---

## Supported Agents

Claude Code · Cursor · Windsurf · GitHub Copilot · Gemini CLI · Aider · Continue · and 30+ more via MCP.

---

## Community & Feedback

- **Discord:** [Join the community](https://discord.gg/weave)
- **In-terminal feedback:** Run `/weave feedback` to send feedback directly from your workflow
- **Issues:** [GitHub Issues](https://github.com/weave-ai-dev/agent-skills/issues)

---

## License

MIT

---

**Plan. Review. Execute.**
The bridge between AI intent and human control.

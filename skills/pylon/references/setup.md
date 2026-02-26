# Weave MCP Setup

## Prerequisites

1. A running Weave instance (self-hosted or cloud)
2. An auth token — generate one at **Settings > Auth Tokens** in the Weave web UI

## Claude Code

Add to `.mcp.json` in your project root (or `~/.claude/.mcp.json` for global):

```json
{
  "mcpServers": {
    "weave": {
      "type": "streamable-http",
      "url": "https://your-weave-instance.com/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

Or via CLI:
```bash
claude mcp add weave --transport streamable-http \
  --url "https://your-weave-instance.com/api/mcp" \
  --header "Authorization: Bearer YOUR_TOKEN"
```

## OpenAI Codex

Add to `codex.toml` in your project root (or `~/.codex/codex.toml` for global):

```toml
[mcp_servers.weave]
type = "streamable-http"
url = "https://your-weave-instance.com/api/mcp"
bearer_token_env_var = "WEAVE_TOKEN"
```

Set the environment variable:
```bash
export WEAVE_TOKEN="YOUR_TOKEN"
```

## Cursor / Windsurf / Generic MCP Client

Set the environment variable and configure the MCP endpoint:

```
MCP_ENDPOINT=https://your-weave-instance.com/api/mcp
MCP_AUTH_TOKEN=YOUR_TOKEN
```

Refer to your agent's MCP documentation for the exact config file format.

## Verifying the Connection

After setup, ask your agent to run:
```
list_documents()
```

If it returns a list (even empty), the connection is working.

## Local Development

For local Weave instances:
```json
{
  "mcpServers": {
    "weave": {
      "type": "streamable-http",
      "url": "http://localhost:3000/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

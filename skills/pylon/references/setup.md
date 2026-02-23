# Pylon MCP Setup

## Prerequisites

1. A running Pylon instance (self-hosted or cloud)
2. An auth token — generate one at **Settings > Auth Tokens** in the Pylon web UI

## Claude Code

Add to `.mcp.json` in your project root (or `~/.claude/.mcp.json` for global):

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

Or via CLI:
```bash
claude mcp add pylon --transport streamable-http \
  --url "https://your-pylon-instance.com/api/mcp" \
  --header "Authorization: Bearer YOUR_TOKEN"
```

## Cursor / Windsurf / Generic MCP Client

Set the environment variable and configure the MCP endpoint:

```
MCP_ENDPOINT=https://your-pylon-instance.com/api/mcp
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

For local Pylon instances:
```json
{
  "mcpServers": {
    "pylon": {
      "type": "streamable-http",
      "url": "http://localhost:3000/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

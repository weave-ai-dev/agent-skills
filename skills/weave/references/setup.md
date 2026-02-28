# Weave MCP Setup

## Prerequisites

1. A running Weave instance (self-hosted or cloud)
2. A Weave account — [sign up here](https://useweave.dev)

Authentication is handled automatically via OAuth. When your tool first connects, it will open a browser window for you to sign in and approve access. No tokens or API keys needed.

## Claude Code

Run this command in your terminal:

```bash
claude mcp add --transport http weave https://weave-dev.com/api/mcp
```

Then execute `/mcp` in a Claude Code session to authenticate.

Or add to `.mcp.json` in your project root (or `~/.claude/.mcp.json` for global):

```json
{
  "mcpServers": {
    "weave": {
      "type": "url",
      "url": "https://weave-dev.com/api/mcp"
    }
  }
}
```

## Cursor

Add to your MCP server configuration:

```json
{
  "mcpServers": {
    "weave": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://weave-dev.com/api/mcp"]
    }
  }
}
```

## Windsurf

Access settings with `CTRL/CMD + ,`, navigate to Cascade > MCP servers, then add custom server:

```json
{
  "mcpServers": {
    "weave": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://weave-dev.com/api/mcp"]
    }
  }
}
```

## Visual Studio Code

1. Press `CTRL/CMD + P`, search "MCP: Add Server"
2. Select "Command (stdio)"
3. Enter: `npx mcp-remote https://weave-dev.com/api/mcp`
4. Name it "Weave"
5. Activate via "MCP: List Servers"

## Zed

Open settings with `CMD + ,` and add:

```json
{
  "context_servers": {
    "weave": {
      "source": "custom",
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://weave-dev.com/api/mcp"],
      "env": {}
    }
  }
}
```

## Other Clients

For any MCP-compatible client, use:

- **Command**: `npx`
- **Arguments**: `-y mcp-remote https://weave-dev.com/api/mcp`

Refer to your agent's MCP documentation for the exact config file format.

## Verifying the Connection

After setup, ask your agent to run:
```
list_documents()
```

If it returns a list (even empty), the connection is working.

## Local Development

For local Weave instances, use `http://localhost:3000/api/mcp` instead of the production URL:

**Claude Code:**
```json
{
  "mcpServers": {
    "weave": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "http://localhost:3000/api/mcp"]
    }
  }
}
```

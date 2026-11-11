# Installing the Job Alerts MCP server

This is a hosted (remote) MCP server. There is no package to install and no
process to run. Add the URL to the client's MCP configuration.

- Transport: Streamable HTTP
- URL: `https://recruiting.herakraft.co/mcp`
- Authentication: none required. Optional `Authorization: Bearer ja_live_...`
  header for higher limits (keys at https://recruiting.herakraft.co/developers/dashboard).
- Environment variables: none.

## Cline / Roo Code / any `mcpServers` JSON

Add to the MCP settings file:

```json
{
  "mcpServers": {
    "job-alerts": {
      "type": "streamableHttp",
      "url": "https://recruiting.herakraft.co/mcp"
    }
  }
}
```

Clients that use `"transport": "http"` or `"type": "http"` take the same URL.

## Claude Code

```bash
claude mcp add --scope user --transport http job-alerts https://recruiting.herakraft.co/mcp
```

## Clients without remote support (stdio only)

```json
{
  "mcpServers": {
    "job-alerts": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://recruiting.herakraft.co/mcp"]
    }
  }
}
```

## Verify

Call `list_tracks` with no arguments. It returns track slugs with open-role
counts and needs no key. If it answers with a `rate_limited` error, wait a
minute; the keyless limit is per IP per minute.

## Troubleshooting

- `401` or `invalid_key`: the Authorization header holds something other than a
  live key. Remove the header; the server works without one.
- `plan_required` from `recent_changes`: that tool needs a Realtime key. The
  other five tools do not.

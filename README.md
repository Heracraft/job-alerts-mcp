# Job Alerts MCP server

A remote, read-only [MCP](https://modelcontextprotocol.io) server over a catalog
of open **US internship and new-grad roles**, read from employers' own career
sites (Workday, Greenhouse, Ashby, Lever, SmartRecruiters, Oracle and others).
Each role carries its track, level, location, visa sponsorship reading,
citizenship or clearance restriction, and the graduation window the posting asks
for. Senior and mid-level roles are not in the catalog.

- Endpoint: `https://recruiting.herakraft.co/mcp` (Streamable HTTP)
- Works **without a key**. A key raises the rate limit and page size; a Realtime
  key removes the freshness delay and adds `recent_changes`.
- Docs: https://recruiting.herakraft.co/developers/docs/mcp
- Server card: https://recruiting.herakraft.co/.well-known/mcp.json
- REST equivalent and `llms.txt`: https://recruiting.herakraft.co/llms.txt
- Privacy: https://recruiting.herakraft.co/privacy

This repository holds the listing manifests (official MCP Registry
`server.json`, Gemini CLI extension, Agent Plugins `plugin.json`, Docker MCP
Catalog entry). The server itself is hosted; there is nothing to run.

## Connect

**Claude Code**

```bash
claude mcp add --scope user --transport http job-alerts https://recruiting.herakraft.co/mcp
```

**Cursor, Windsurf, VS Code, any `mcpServers` config**

```json
{
  "mcpServers": {
    "job-alerts": { "type": "http", "url": "https://recruiting.herakraft.co/mcp" }
  }
}
```

**Claude Desktop** (no native remote support; goes through `mcp-remote`)

```json
{
  "mcpServers": {
    "job-alerts": { "command": "npx", "args": ["-y", "mcp-remote", "https://recruiting.herakraft.co/mcp"] }
  }
}
```

**Gemini CLI**

```bash
gemini extensions install https://github.com/Heracraft/job-alerts-mcp
```

**claude.ai** (Settings, Connectors, Add custom connector): paste the endpoint URL. No sign-in.

With a key, send `Authorization: Bearer ja_live_...` as a header. Keys come from
https://recruiting.herakraft.co/developers/dashboard.

## Tools

| Tool | What it returns |
| --- | --- |
| `search_roles` | Open roles, newest first, with `next_cursor` paging. Filters: `query`, `track`, `level`, `group`, `company`, `exclude_companies`, `country`, `workplace`, `us_only`, `include_clearance`, `sponsorship`, `citizenship`, `undergrad`, `graduation_year`, `since`, `posted_after`. |
| `get_role` | One role by id, open or closed. |
| `list_tracks` | Track slugs (`swe`, `data-ai`, `quant`, ...) with open-role counts. |
| `list_groups` | Company groups: curated lists (`faang`, `ai-companies`, `quant-hft`, ...) and sectors. |
| `list_companies` | Companies with open roles, most first. |
| `recent_changes` | Feed of roles announced and closed. Realtime key only. |

Every tool is annotated `readOnlyHint: true`. Results default to roles open to
US applicants that do not require citizenship or a clearance; pass
`us_only=false`, a `country`, or `include_clearance=true` to widen. Send people
to a role's `apply_url`; posting text is not served.

## Example

> Find software engineering internships for summer 2027 that sponsor visas, at AI companies.

The agent calls `search_roles` with `track: "swe"`, `level: "intern"`,
`group: "ai-companies"`, `sponsorship: "available"`, `graduation_year: 2027`
and reads back titles, companies, locations and apply links.

## Plans

Free is keyless and one hour behind. Realtime is a paid key with no delay,
evidence sentences behind each eligibility field, and the `recent_changes` feed.
Limits and terms: https://recruiting.herakraft.co/developers/pricing

## Files here

| File | For |
| --- | --- |
| `server.json` | Official MCP Registry (`mcp-publisher publish`) |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI extensions gallery |
| `plugin.json`, `mcp.json` | Agent Plugins format (cursor.directory, Cursor marketplace) |
| `docker/` | Docker MCP Catalog entry (`docker/mcp-registry`) |
| `llms-install.md` | Agent-readable install notes (Cline marketplace) |
| `glama.json` | Glama ownership |

MIT licensed. The hosted service has its own [terms](https://recruiting.herakraft.co/developers/pricing#terms).

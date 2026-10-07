# Better Stack plugin for Codex

Connect [Codex](https://developers.openai.com/codex/) to your [Better Stack](https://betterstack.com) Incidents and Telemetry data through the Model Context Protocol (MCP). Your agent can investigate incidents, check who is on call, manage uptime monitors, query logs, metrics, traces and errors, and build dashboards, all in natural language.

## Install

This repository is also a Codex plugin marketplace. Add it, then install the plugin:

```bash
codex plugin marketplace add BetterStackHQ/codex-plugin
codex plugin add betterstack@betterstack
```

Or add the MCP server directly to `~/.codex/config.toml`:

```toml
[mcp_servers.betterstack]
url = "https://mcp.betterstack.com"
```

Then sign in with OAuth (opens a browser, no token needed):

```bash
codex mcp login betterstack
```

## What you can do

Try asking your agent things like:

- *"Show me all monitors that are currently down."*
- *"What's the availability of my website this month?"*
- *"What incidents occurred yesterday?"*
- *"Who's on-call right now?"*
- *"Acknowledge incident #1234 and add a comment about the fix."*
- *"Build an explore query to find HTTP 500 errors in the last hour."*
- *"Create a dashboard showing error rates for my API service."*

## Tools

The plugin exposes the full Better Stack MCP toolset:

- **Incidents** (formerly Uptime): incidents, on-call schedules and escalation, uptime monitors, heartbeats, status pages.
- **Telemetry**: dashboards, charts, alerts, log/metric/error queries, sources and applications.
- **Documentation**: search Better Stack docs from within Codex.

The complete tool reference and example prompts live in the [Better Stack MCP integration docs](https://betterstack.com/docs/getting-started/integrations/mcp/).

## Authentication

OAuth is the recommended flow. Run `codex mcp login betterstack` to sign in through your browser. No token configuration needed.

If you prefer an API token, set one via an environment variable. Get a Better Stack [API token](https://betterstack.com/docs/uptime/api/getting-started-with-uptime-api/), then reference it from `config.toml`:

```toml
[mcp_servers.betterstack]
url = "https://mcp.betterstack.com"
bearer_token_env_var = "BETTERSTACK_API_TOKEN"
```

## Limiting available tools

Restrict which tools the agent can use with one of these headers:

- `X-MCP-Tools-Only`: allowlist (only the listed tools are available)
- `X-MCP-Tools-Except`: blocklist (all tools except the listed ones)

```toml
[mcp_servers.betterstack]
url = "https://mcp.betterstack.com"
http_headers = { "X-MCP-Tools-Only" = "monitors,monitor,incidents,incident" }
```

Useful for giving your agent read-only access, scoping it to a workflow, or trimming the initial context size.

## License

MIT. See [LICENSE](LICENSE).

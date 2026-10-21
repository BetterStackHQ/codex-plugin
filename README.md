# Better Stack plugin for Codex

Connect [Codex](https://developers.openai.com/codex/) to your [Better Stack](https://betterstack.com) Incidents and Telemetry data through the Model Context Protocol (MCP). Your agent can investigate incidents, check who is on call, manage uptime monitors, query logs, metrics, traces, errors and real user monitoring (RUM), and build dashboards, all in natural language.

## Install

### ChatGPT

Open **Plugins** in the sidebar, find **Better Stack** and select the plus button to install it. Connect Better Stack when ChatGPT asks and sign in with your Better Stack account. Then start a new chat.

### Codex

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

Either way, sign in with OAuth (opens a browser, no token needed), then start a new Codex session:

```bash
codex mcp login betterstack
```

`codex mcp list` shows whether you're signed in.

### Not connected yet?

The plugin's **connect-betterstack** skill is its setup step. ChatGPT and Codex offer it after you install the plugin, and the agent also uses it when a Better Stack request comes in but the Better Stack tools aren't connected. It tells you how to connect in the app you're using instead of failing quietly.

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

## Skills

The plugin comes with skills that teach your agent how to work with Better Stack. It loads each one when the task calls for it.

**Investigate**

- **investigate-incident**: investigates an incident or alert end to end. It finds the incident, checks who is on call, pulls the errors, logs, traces, metrics and releases around the start time, and posts a short situation report. It stays read-only unless asked to act.
- **health-check**: a quick health assessment of a server or service (CPU, memory, disk, network, log levels, dependencies) with charts.

**Query**

- **logs-query**, **spans-query**, **metrics-query**, **errors-query**: the SQL dialect for each data type, with the mistakes that make queries fail or return wrong numbers.
- **collector-queries**: proven host, container and service queries for Better Stack Collector sources.

**Dashboards and alerts**

- **dashboard-builder**, **dashboard-layout**: build dashboards that read well, with template variables and a clean grid.
- **chart-generation**, **chart-styling**, **map-hierarchy-charts**: chart types, units and settings.
- **chart-alerts**: threshold, relative and anomaly alerts on charts.
- **uptime-monitors**: monitor types, valid settings and escalation policies.

**Migrate**

- **grafana-import**: recreate Grafana dashboards in Better Stack.

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

## Maintainers

- **One version for all three plugins.** The Claude ([claude-plugin](https://github.com/BetterStackHQ/claude-plugin)), Codex ([codex-plugin](https://github.com/BetterStackHQ/codex-plugin)) and Cursor ([cursor-plugin](https://github.com/BetterStackHQ/cursor-plugin)) plugins always release with the same version number. A change that bumps one plugin's version bumps all three to that same number, in PRs opened together, even when the other two have nothing else to change. The version is set in `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` (Claude), `.codex-plugin/plugin.json` (Codex) and `.cursor-plugin/plugin.json` (Cursor).
- **Skills from AI SRE.** All skills except `investigate-incident` and `connect-betterstack` are built from AI SRE's skills in aisre-proxy with the sync tool in the maintainers' repository, `BetterStackHQ/mcp-plugins` (`sync-skills/`). Change them in aisre-proxy, then sync.

## License

MIT. See [LICENSE](LICENSE).

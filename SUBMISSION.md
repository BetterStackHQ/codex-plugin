# Plugins Directory submission checklist

Requirements from the [official submission guide](https://developers.openai.com/plugins/deploy/submission)
that live OUTSIDE this repo. The repo itself (manifest, marketplace file, logo, MCP wiring)
is submission-ready; these are portal-side and server-side prerequisites.

## Import file

`chatgpt-app-submission.json` (repo root) follows the official schema
(https://developers.openai.com/plugins/schemas/chatgpt-app-submission.v1.json) from the
`chatgpt-app-submission` skill in OpenAI's [openai/plugins](https://github.com/openai/plugins)
"OpenAI Developers" plugin. Upload it in the submission form to prefill App Info, tool
annotations + justifications, and the 5 positive / 3 negative test cases. It was generated
from the LIVE `tools/list` of mcp.betterstack.com (116 tools, all with explicit hints).
Justifications containing `TODO(mcp-team)` mark declared hints that look inconsistent with
Apps SDK review semantics (see PR notes) and need confirming before submission.

## Server-side (mcp.betterstack.com)

- [x] Tool annotations on every MCP tool: all 116 tools declare `readOnlyHint`,
      `openWorldHint`, and `destructiveHint` explicitly (verified via live `tools/list`).
- [ ] Resolve the `TODO(mcp-team)` hint questions in `chatgpt-app-submission.json`
      (status-page writes declare `openWorldHint=false` despite editing public pages;
      `query`/`render_chart`/`create_dashboard`/`import_dashboard` declare it `true`;
      `invite_team_member` sends e-mail but declares it `false`).
- [ ] `outputSchema` is missing on all 116 tools - not a blocker, but recommended so
      models can use results more reliably.
- [ ] Domain verification token served at `/.well-known/openai-apps-challenge`.
- [ ] Content security policy: declare the exact domains the server fetches.

## Portal-side (submission form)

- [ ] Submission type: "With MCP" (this plugin is MCP-only, no bundled skills).
- [ ] Verified developer identity (business).
- [ ] Demo credentials for a test account that works without MFA, SMS, email
      confirmation, or private-network access.
- [x] 5 positive + 3 negative test cases: written out in
      `chatgpt-app-submission.json`; attach the demo test account details in the form.
- [ ] Support URL, availability countries, release notes, policy attestations.

Listing metadata (name, descriptions, category, logo, website, privacy, terms,
starter prompts) mirrors `.codex-plugin/plugin.json` -> `interface`.

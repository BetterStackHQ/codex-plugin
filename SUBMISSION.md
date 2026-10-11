# Plugins Directory submission checklist

Requirements from the [official submission guide](https://developers.openai.com/plugins/deploy/submission)
that live OUTSIDE this repo. The repo itself (manifest, marketplace file, logo, MCP wiring)
is submission-ready; these are portal-side and server-side prerequisites.

## Server-side (mcp.betterstack.com)

- [ ] Tool annotations on every MCP tool: `readOnlyHint` (true for fetch/list),
      `openWorldHint` (true if it changes publicly visible internet state),
      `destructiveHint` (true if irreversible). Required for the portal's "Scan Tools" step.
- [ ] Domain verification token served at `/.well-known/openai-apps-challenge`.
- [ ] Content security policy: declare the exact domains the server fetches.

## Portal-side (submission form)

- [ ] Submission type: "With MCP" (MCP server plus bundled skills).
- [ ] Verified developer identity (business).
- [ ] Demo credentials for a test account that works without MFA, SMS, email
      confirmation, or private-network access.
- [x] Test cases: 5 positive and 3 negative, in `.codex-plugin/plugin.json` under
      `extensions.com.openai.review.test_cases`. The portal imports them read-only from the ZIP,
      so change them here and upload a new ZIP.
- [ ] Demo recording URL (`review.demo_recording_url`, can also go in the manifest).
- [x] Support URL: `interface.supportURL`.
- [ ] Availability countries, release notes, policy attestations.

Listing metadata (name, descriptions, category, logo, website, privacy, terms,
starter prompts) mirrors `.codex-plugin/plugin.json` -> `interface`.

## Updating the ChatGPT plugin

ChatGPT doesn't read this repository. The Better Stack plugin in ChatGPT shows the skills
and icons from the last ZIP uploaded in the portal, and its MCP connection is the one set up
on the portal's MCPs tab. To ship a change from this repo, upload a new ZIP to the existing
plugin. It gets its own checks and review, and replaces the previous version once published.

Build the ZIP with `scripts/chatgpt-zip` (defaults: `main`, `betterstack-plugin.zip`). It leaves out
the marketplace file, this checklist and the scripts (`.gitattributes`), and renames the plugin to
`app-6aa02bd506908191bbf12fc2ec2f3161` inside the ZIP only: the portal requires the live plugin's
name, while Codex installs it as `betterstack`.

```bash
scripts/chatgpt-zip main betterstack-plugin.zip
```

Upload it from inside the live plugin, not from the Plugins list, which creates a new plugin:
open https://platform.openai.com/plugins/manage/plugin_asdk_app_6aa02bd506908191bbf12fc2ec2f3161
(Better Stack's OpenAI organization only) and upload the ZIP there.

The manifest sets `extensions.com.openai.onboardingSkill` to the connect-betterstack skill,
so ChatGPT and Codex offer it as the plugin's setup step after install.

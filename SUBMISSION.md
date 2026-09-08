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

- [ ] Submission type: "With MCP" (this plugin is MCP-only, no bundled skills).
- [ ] Verified developer identity (business).
- [ ] Demo credentials for a test account that works without MFA, SMS, email
      confirmation, or private-network access.
- [ ] At least 5 positive test cases (prompt, expected behavior, result shape,
      test account) and 3 negative test cases (prompt, expected refusal or safe
      fallback, justification).
- [ ] Support URL, availability countries, release notes, policy attestations.

Listing metadata (name, descriptions, category, logo, website, privacy, terms,
starter prompts) mirrors `.codex-plugin/plugin.json` -> `interface`.

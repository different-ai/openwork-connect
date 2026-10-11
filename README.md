# OpenWork Connect

One OAuth MCP connection for the skills, reusable work and connected tools assigned to you in your OpenWork workspace.

This repository contains only the installation package and repository-marketplace catalogs. The gateway is hosted at:

```text
https://api.openworklabs.com/mcp/agent
```

You need an OpenWork account and workspace access. Your client handles browser OAuth; OpenWork checks the selected workspace, membership and grants. Installation does not complete authentication or connect downstream services.

No credentials, backend code, dependencies, launchers or hooks are bundled. Never add bearer headers, API keys, client secrets or tokens to these files or an install link.

## Package formats

| Client/format | Package entry | Repository catalog |
| --- | --- | --- |
| Portable Agent Plugins / supported Codex clients | `plugin.json`, `mcp.json`, `skills/` | `.agents/plugins/marketplace.json` |
| Claude-compatible clients | `.claude-plugin/plugin.json`, `.mcp.json` | `.claude-plugin/marketplace.json` |
| Native Cursor Plugins | `.cursor-plugin/plugin.json`, explicitly selecting `.mcp.json` | `.cursor-plugin/marketplace.json` |

All formats point to the same endpoint and share `skills/openwork-connect/SKILL.md`. Portable transport is `streamable-http`; the separate native configuration uses `http`. Neither pins a wire-protocol revision or contains authentication secrets. Catalog sources point to `./`, this repository's root.

## Install

### Cursor

Use Cursor's supported plugin repository/import workflow and select:

```text
https://github.com/different-ai/openwork-connect
```

Confirm exactly one intended OpenWork connection and complete browser OAuth. Check the installed version is **1.0.1** before testing its guidance. Adding a local repository again did not replace an existing 1.0.0 package in the reported Cursor 3.24.12 test; do not assume an import toast proves an update. Preserve existing settings and credentials and use the client's documented update mechanism.

### Claude Code

```sh
claude plugin marketplace add different-ai/openwork-connect
claude plugin install openwork-connect@openwork
```

Start a new session, inspect `/mcp`, and authenticate the OpenWork server. Other Claude surfaces have their own plugin/connector workflows and account requirements.

For strict directory-field and MCP validation, use Claude Code **2.1.281+**. Earlier versions warn on the directory-only `icon` field and fail `--strict`, even though loading can strip that field and installation can succeed.

### Codex

```sh
codex plugin marketplace add different-ai/openwork-connect
codex plugin marketplace list
```

Install through a supported plugin interface. In the tested CLI **0.162.0-alpha.17.2**, installation uses:

```sh
codex plugin add openwork-connect@openwork
codex mcp login openwork
```

Confirm the selected server comes from the plugin before login. A manual/config MCP entry named `openwork` overrides the plugin's same-name server **even when disabled**. Use an isolated test context with no same-name manual registration; do not enable the manual entry and call that a bundled-server test.

Inspect `/plugins`, `/mcp verbose` and `/warnings`. Tool policies use raw names such as `list_skills` and `get_skill`, and MCP tools may be deferred behind native tool search. Installation and the catalog's `ON_INSTALL` policy do not themselves perform CLI OAuth. Do not copy another profile's credentials or blindly add authentication extensions unsupported by your client version.

## Try it

Ask for a skill or connected outcome that is actually assigned to your workspace. The bundled skill guides discovery and exact invocation.

For a specifically named skill, verify the requested reference with `get_skill`. Ranked search results are relevance candidates, not proof that the requested name exists. If the exact reader returns `unknown_skill`, report that it is unavailable to the current member; do not relabel or silently execute another result.

Tools, resources and downstream services vary by workspace, grants and connection readiness. A populated test workspace is not proof that every operation or client surface works.

## Verification and limits

Version 1.0.1 originates from [OpenWork PR #5850](https://github.com/different-ai/openwork/pull/5850), commit `1aecf7e50d5eda95d4c918d0f18a3e5d79c4bc59`. This standalone repository changes repository metadata and this README; MCP configurations, the shared skill, license and logo retain the source payload.

- Source packaging checks passed: 20 dependency-free tests and Claude Code 2.1.284 strict validation.
- Reported Codex CLI 0.162.0-alpha.17.2 QA passed bundled reader connection, fixture retrieval and exact missing-skill refusal in an isolated context.
- Reported Cursor 3.24.12 QA passed positive and negative skill-guidance cases after an approved manual replacement of the QA package. Normal local-import upgrade and fresh consent were not established by that test.
- Token refresh, rejected use of a revoked server token, cross-workspace isolation and every supported client surface remain incomplete in that native report.
- Installation from this newly published Git repository requires its own verification; prior local-export testing is not that proof.

**Repository marketplaces are not public-store approvals.** No Cursor, Claude or OpenAI marketplace approval is claimed. The current general gateway does not meet OpenAI's public-directory rule requiring individually exposed operations instead of discovery plus a generic executor. Codex private/repository use is separate from that public review route.

## Documentation and support

- [Connect OpenWork MCP](https://openworklabs.com/docs/start-here/connect-openwork-mcp)
- [Support](https://openworklabs.com/contact)
- [Privacy](https://openworklabs.com/privacy)
- [Terms](https://openworklabs.com/terms)
- [OpenWork source](https://github.com/different-ai/openwork)

## License

MIT; see `LICENSE`. The logo is copied unchanged from OpenWork's MIT desktop asset `apps/app/public/openwork-logo-square.svg`. This package license does not relicense the hosted service or OpenWork Enterprise Edition backend code.

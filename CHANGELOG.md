# Changelog

## [1.1.0] - 2026-09-23

### Changed
- Web research works with the `harness-web` MCP server (`mcp__harness-web__websearch`, `mcp__harness-web__webfetch`) as well as the built-in WebSearch and WebFetch. The agent uses whichever is present and stops with a clear message when neither is.
- The enhancement pass is off by default. Turn it on with `--enhance`; it is skipped silently when the enhance plugin is not installed. `--no-enhance` is still accepted.
- `learn-agent` inherits the caller's model instead of pinning `sonnet`.
- The recent-developments query no longer hardcodes years.
- Agent constraints are stated once each with their reason, without all-caps pressure wording.
- `/learn` on a harness without AskUserQuestion updates an existing guide instead of stalling.

## [1.0.0] - 2026-02-21

Initial release. Extracted from [agentsys](https://github.com/agent-sh/agentsys) monorepo.

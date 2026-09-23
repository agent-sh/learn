# Changelog

## [1.2.0] - 2026-09-24

### Changed
- Rewrote the `/learn` command, `learn-agent` and the `learn` skill for current models: goal, constraints with their reasons, a definition of done and the output contract, in place of phase-by-phase pseudocode and repeated rules.
- The skill is shorter; the source-quality scale and the guide, index and sources layouts moved to `skills/learn/references/`.
- `learn-agent` runs on `sonnet`. The 1.1.0 entry said it inherits the caller's model; the shipped 1.1.0 file already pinned `sonnet`.
- Without the Task tool, `/learn` runs the skill in the calling session. Without the Skill tool, the agent reads the skill file directly.
- The `/learn` reply shows `Accuracy`, the field the agent actually returns, instead of a `Confidence` field that never existed.
- Unchanged: command name, arguments, depth levels, slug rule, output paths, the sources JSON fields and the `LEARN_RESULT` block.

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

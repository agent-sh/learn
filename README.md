# learn

Research any topic online and create comprehensive learning guides with RAG-optimized indexes for your AI agents.

## Why

AI agents work better when they have curated, pre-researched knowledge to draw from instead of searching the web on every question. `/learn` builds that knowledge base systematically - gathering sources, scoring them for quality, and synthesizing structured guides that agents can reference instantly.

Use cases:

- Learning a new technology before starting implementation
- Building a shared knowledge base across a team's AI tools
- Creating authoritative reference material from scattered online sources
- Producing guides that work as RAG context for Claude Code, OpenCode, and Codex

## Installation

```bash
agentsys install learn
```

Requires [agentsys](https://github.com/agent-sh/agentsys) to be set up in your project.

## Quick Start

```
/learn react hooks
```

This searches the web for ~20 sources on React hooks, scores each source for authority and depth, fetches the top results, and writes a synthesized guide to `agent-knowledge/react-hooks.md` with a companion `resources/react-hooks-sources.json` containing full source metadata.

## How It Works

The learn skill follows a six-stage methodology:

1. **Progressive discovery** - Funnel approach: broad queries for landscape mapping, focused queries for core content, deep queries for advanced material. Avoids noise from dumping all queries at once.

2. **Quality scoring** - Each source is scored on a 100-point scale across five dimensions: authority (3x weight), recency (2x), depth (2x), examples (2x), and uniqueness (1x). Official docs score highest; undated blog posts score lowest.

3. **Just-in-time extraction** - Only high-scoring sources get fetched. Summaries and key insights are extracted - never full content. This keeps token usage predictable and respects copyright.

4. **Synthesis** - A structured learning guide is generated with prerequisites, core concepts, code examples, common pitfalls, best practices, and further reading. Content is cross-referenced across sources, not copied from any single one.

5. **RAG index** - The master index (`agent-knowledge/CLAUDE.md` and `AGENTS.md`) is updated with the new topic, trigger phrases, and keyword mappings so agents can find relevant guides automatically.

6. **Enhancement** - Runs `enhance:enhance-docs` and `enhance:enhance-prompts` on the output to improve RAG retrieval quality. Skip with `--no-enhance`.

## Usage

```bash
# Default depth (20 sources)
/learn recursion

# Deep research (40 sources)
/learn kubernetes networking --depth=deep

# Quick overview (10 sources)
/learn python decorators --depth=brief

# Skip enhancement pass
/learn typescript generics --no-enhance
```

### Depth Levels

| Level | Sources | When to Use |
|-------|---------|-------------|
| `brief` | 10 | Quick overview, time-sensitive topics |
| `medium` | 20 | Balanced coverage (default) |
| `deep` | 40 | Comprehensive research, complex topics |

### Output Files

Each run creates or updates:

```
agent-knowledge/
  CLAUDE.md                       # Master index (updated)
  AGENTS.md                       # Master index for OpenCode/Codex (updated)
  <topic-slug>.md                 # Synthesized learning guide
  resources/
    <topic-slug>-sources.json     # Source metadata with quality scores
```

### Existing Topics

If a guide already exists for the topic, you are prompted to either update the existing guide with new sources or start fresh.

## Architecture

| Component | Type | Model | Role |
|-----------|------|-------|------|
| `learn` | command | - | Entry point, argument parsing |
| `learn-agent` | agent | sonnet | Research coordination, web search, synthesis |
| `learn` | skill | - | Research methodology, scoring rubric, templates |

## Requirements

- [agentsys](https://github.com/agent-sh/agentsys) runtime
- Web access (WebSearch and WebFetch tools)
- An `agent-knowledge/` directory in the workspace (created automatically)

## Related Plugins

- [agent-knowledge](https://github.com/agent-sh/agent-knowledge) - Where guides are stored; contains existing research
- [enhance](https://github.com/agent-sh/enhance) - Post-processing for RAG optimization
- [consult](https://github.com/agent-sh/consult) - For getting a second opinion on specific questions instead of building a full guide

## License

MIT

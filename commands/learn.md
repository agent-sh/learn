---
description: Use when user asks to "learn about topic", "research subject", "create learning guide", "build knowledge base", "study topic", or wants to gather online resources on any subject.
codex-description: 'Use when user asks to "learn about topic", "research subject", "create learning guide", "build knowledge base", "study topic". Gathers online sources and synthesizes comprehensive guide with RAG index.'
argument-hint: "[topic] [--depth=brief|medium|deep] [--enhance]"
allowed-tools: Task, Read, Write, Glob, AskUserQuestion
---

# /learn

Research a topic online and write a cited learning guide to `agent-knowledge/{slug}.md`, with source metadata and a RAG index that later sessions read.

## Arguments

From `$ARGUMENTS`:

- **topic**: everything that is not a flag. Required. With no topic, reply `Usage: /learn <topic> [--depth=brief|medium|deep] [--enhance]` and stop.
- **--depth**: `brief` (10 sources), `medium` (20, default), `deep` (40).
- **--enhance**: run the optional enhancement pass. Off by default, skipped when the `enhance` plugin is not installed. `--no-enhance` is accepted and means the default.

**slug**: the topic lowercased, characters other than `a-z`, `0-9`, space and `-` removed, runs of spaces and dashes collapsed to one `-`, leading and trailing `-` trimmed, cut to 64 characters. Existing guides are found by this name, so derive it exactly.

## Existing guide

If `agent-knowledge/{slug}.md` exists, ask whether to update it (add new sources, refresh content) or start fresh. Without AskUserQuestion, update it and say so in the reply: an update keeps the user's earlier work, a fresh run discards it.

## Run

Spawn `learn:learn-agent` with:

```
Research and create a learning guide.

Topic: {topic}
Slug: {slug}
Depth: {depth}
Min Sources: {10|20|40}
Enhance: {true|false}
Existing guide: {update|fresh|none}

Output directory: agent-knowledge/
Return structured results between === LEARN_RESULT === markers.
```

Without the Task tool, do the same work in this session: load the `learn` skill (or read the plugin's `skills/learn/SKILL.md`) with the same inputs.

Read the JSON between `=== LEARN_RESULT ===` and `=== END_RESULT ===`. If it is missing or does not parse, report what the agent returned and which files exist; do not invent numbers.

## Reply

```markdown
## Learning Guide Created

**Topic**: {topic}
**File**: agent-knowledge/{slug}.md
**Sources**: {sourceCount} resources analyzed

| Metric | Rating |
|--------|--------|
| Coverage | {coverage}/10 |
| Source Diversity | {diversity}/10 |
| Example Quality | {examples}/10 |
| Accuracy | {accuracy}/10 |

| Type | Count |
|------|-------|
| Official Docs | {officialDocs} |
| Tutorials | {tutorials} |
| Q&A | {stackOverflow} |
| Blog Posts | {blogPosts} |
| GitHub | {github} |

**Gaps**: {gaps, or "none"}
```

Add a line when the source target was not met, the guide was updated rather than created, or enhancement was asked for and skipped.

## Files written

```
agent-knowledge/
  CLAUDE.md                  # master index (updated)
  AGENTS.md                  # same index for Codex and OpenCode (updated)
  {slug}.md                  # the guide
  resources/{slug}-sources.json
```

Examples: `/learn recursion`, `/learn react hooks --depth=deep`, `/learn "kubernetes networking" --depth=brief`, `/learn python async --enhance`.

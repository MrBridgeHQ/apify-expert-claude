# Apify Expert - a Claude skill

A Claude Code skill that serves as a **platform-knowledge hub** for the [Apify](https://apify.com) platform: a curated, deduplicated baseline of platform mechanics (Actors, runs, builds, storage, Apify Proxy, schedules, webhooks, integrations, the REST API, the `apify` CLI, the JS/Python SDK, Standby mode, Store mechanics) plus a newest-first digest of recent platform changes.

It exists so that any Claude Code session - whether you're building a scraper, an MCP server, pricing an Actor, or writing Store content - has one authoritative place to check **current** Apify platform facts instead of relying on stale memory.

## Why this skill exists

Platform facts drift. A document that hard-codes "Standby idle timeout default is X" or "templates are A/B/C" goes stale silently, and an LLM answering from training data will confidently quote a number that changed six months ago. This skill is the one place that stays curated: the `references/*` files are the deduplicated platform baseline, and `references/changelog-digest.md` tracks recent changes from the public [Apify change-log](https://apify.com/change-log). Everything else cross-references here instead of duplicating (and risking divergence).

## What it covers

| Topic | Reference file |
|---|---|
| Actors, runs, builds, versioning/tags, memory↔CPU, resurrection, storage, proxy, schedules, webhooks | `references/platform-overview.md` |
| `apify` CLI commands, JS/Python SDK, `apify create` templates | `references/cli-and-sdk.md` |
| REST API, tokens, Apify MCP access pattern, native integrations | `references/api-and-integrations.md` |
| Standby mode, Store mechanics, account/limits/plans | `references/platform-features.md` |
| Recent platform changes (newest-first, impact-tagged) | `references/changelog-digest.md` |

## What's inside

```
apify-expert/
├── SKILL.md                          # Orchestration hub: what it owns, delegation map, how to consult
├── README.md                         # This file
├── INSTALL.md                        # Installation instructions
└── references/
    ├── platform-overview.md          # Actors / runs / builds / storage / proxy / schedules / webhooks
    ├── cli-and-sdk.md                # apify CLI + JS/Python SDK + create templates
    ├── api-and-integrations.md       # REST API + Apify MCP + native integrations
    ├── platform-features.md          # Standby mode + Store mechanics + account/limits
    └── changelog-digest.md           # Newest-first, impact-tagged digest of recent Apify changes
```

## What this skill does NOT do

This skill owns *platform mechanics*. Deep domain work belongs to dedicated workflows:

- **Scraper Actor code & architecture** (Crawlee, templates, no-code config) - a scraper-building workflow.
- **MCP server Actor code & architecture** (proxy template, Standby wiring) - an MCP-server-building workflow.
- **Pricing / PPE / `Actor.charge` / revenue** - a monetization workflow.
- **README / input-output schemas / Store description** - a Store-content authoring workflow.
- **Anti-bot doctrine, tool ladder, hidden APIs** - a scraping-doctrine workflow.

This skill gives those workflows their *platform substrate* (e.g. "how Standby billing works", "current storage limits") - it does not do their job.

## Installation

See `INSTALL.md`. TL;DR for macOS/Linux:

```bash
mkdir -p ~/.claude/skills
unzip apify-expert.zip -d ~/.claude/skills/
```

This skill has no runtime dependencies - it is pure reference content (Markdown).

## How to invoke

Once installed, the skill auto-activates on Apify platform questions. Example prompts:

- "What's the memory↔CPU coupling on Apify? How many vCPUs does 4096 MB give me?"
- "What's new on Apify? Did the changelog change anything about Standby?"
- "Is the `ts-mcp-server` template still current?"
- "How does Standby billing work - do I pay for idle time?"
- "What are the current `apify create` templates?"

If auto-activation misses, force it: "Use the `apify-expert` skill to..."

## Keeping the changelog digest current

`references/changelog-digest.md` is a manually-maintained, newest-first log. To refresh it, open <https://apify.com/change-log>, find entries newer than the digest's current newest entry, and prepend a formatted, impact-tagged block for each under the `INSERT-NEW-ENTRIES-BELOW` marker (format documented in the digest header). Flag high-impact entries (pricing/billing, API breaks, Standby/MCP behavior, schema/template changes) under "⚠ Needs human review". Full instructions are in `SKILL.md`.

## Part of the mr-bridge.com toolkit

This skill is part of the [mr-bridge.com](https://mr-bridge.com) toolkit for scraping, data, and content automation. Related resources:

- [mr-bridge.com](https://mr-bridge.com) - home
- [Scrapers](https://mr-bridge.com/scrapers) - the Apify Actor portfolio
- [MCP servers](https://mr-bridge.com/mcp-servers) - Model Context Protocol servers
- [AI workflows](https://mr-bridge.com/ai-workflows) - agents and automation
- [Studies](https://mr-bridge.com/studies) - data studies and one-pagers
- [Articles](https://mr-bridge.com/articles) - write-ups and guides
- [Solutions](https://mr-bridge.com/solutions) - end-to-end solutions

## License

Personal use. Customize freely. No warranty. Platform figures flagged *(verify)* should be confirmed against <https://docs.apify.com> before being quoted as hard limits - they drift.

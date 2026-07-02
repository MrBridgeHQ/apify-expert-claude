---
name: apify-expert
description: Use when you need current, authoritative facts about the Apify PLATFORM itself — Actors, runs, builds, storage (Dataset / Key-value store / Request queue), Apify Proxy, schedules, webhooks, integrations, the REST API, the apify CLI, the JS/Python SDK, Standby mode, Store mechanics — or when asked "what's new on Apify", "latest Apify update", "Apify changelog", "did Apify change X", "is feature Y still current". Also the resource that scraper-building, MCP-server-building, monetization, and content-authoring workflows consult for up-to-date platform facts and recent changelog changes. NOT for deep domain work — scraper code, MCP server code, PPE pricing, README/schema authoring — which specialized workflows own; this skill provides their platform substrate.
---

# Apify Expert — platform-knowledge hub & changelog reference

A single source of truth for **Apify platform facts**. This skill owns *platform mechanics*; it **points** deep domain work to the relevant specialized workflow and **flags** (via impact tags in the digest) when a changelog entry touches that domain.

## Core principle

Platform facts drift. A document that hard-codes "Standby idle timeout default is X" or "templates are A/B/C" goes stale silently. This skill is the one place that stays curated — `references/changelog-digest.md` tracks recent changes from <https://apify.com/change-log>, and the reference files are the deduplicated platform baseline. Everything else cross-references here instead of duplicating (and risking divergence).

## What this skill owns

| Topic | Where it lives |
|---|---|
| Actors, runs, builds, versioning/tags, memory↔CPU, resurrection | `references/platform-overview.md` |
| Storage: Dataset / Key-value store / Request queue, retention, limits | `references/platform-overview.md` |
| Apify Proxy: datacenter / residential / SERP, sessions, geo | `references/platform-overview.md` |
| Schedules, webhooks, native integrations, monitoring | `references/platform-overview.md` |
| `apify` CLI commands, JS/Python SDK, `apify create` templates | `references/cli-and-sdk.md` |
| **Building Actors with AI**: assistant prompt, `apify create` templates ship `AGENTS.md`, official `apify/agent-skills`, docs MCP (`?tools=docs`), `llms.txt` | `references/build-with-ai.md` |
| REST API, tokens, scheduling via API, Apify MCP access pattern | `references/api-and-integrations.md` |
| Standby mode platform behavior, Store mechanics, account/limits | `references/platform-features.md` |
| **Recent platform changes** | `references/changelog-digest.md` |

## Delegation map — do NOT answer these here

This skill gives platform substrate (e.g. "how Standby billing works", "current storage limits"). For deep domain work, route to the appropriate specialized workflow:

| If the question is about… | Hand off to… |
|---|---|
| Scraper Actor **code & architecture** (Crawlee, templates, no-code config) | a scraper-building workflow |
| MCP server Actor **code & architecture** (proxy template, Standby wiring) | an MCP-server-building workflow |
| **Pricing / PPE / `Actor.charge` / revenue** | a monetization workflow |
| **README / input-output schemas / Store description / bios** | a Store-content authoring workflow |
| **Anti-bot doctrine, tool ladder, hidden APIs** | a scraping-doctrine workflow |
| **Auditing** an existing scraper | a scraper-audit workflow |

This skill provides those workflows their *platform substrate* — it does not do their job.

## How to consult this skill

Reference **by name**, never by `@path` (which force-loads and burns context):

> For current Apify platform facts and recent changes, see `apify-expert/references/platform-overview.md` and `apify-expert/references/changelog-digest.md`.

When you need to know whether a platform behavior is still current, the digest is the freshness check: an impact-tagged entry newer than your last edit means dependent content may need updating.

## Keeping the changelog digest current

`references/changelog-digest.md` is a manually-maintained, newest-first log of recent Apify platform changes. To refresh it:

1. Open <https://apify.com/change-log>.
2. Identify entries newer than the digest's current newest entry.
3. For each new entry, prepend a formatted, impact-tagged block (see the format in the digest header) directly under the `INSERT-NEW-ENTRIES-BELOW` marker, newest-first.
4. Tag high-impact entries (pricing/billing changes, API breaks, Standby/MCP behavior changes, schema/template changes) under "⚠ Needs human review".

## When a change needs action

High-impact entries get a `⚠ Needs human review` line at the **top** of `changelog-digest.md`. That is the cue to revisit the affected domain content by hand — the digest curates knowledge, it does not rewrite other documents.

## Freshness contract

- `references/*` = curated baseline, edited by hand when a digest entry warrants it.
- `changelog-digest.md` = newest-first log of recent changes.
- If the digest's newest entry is older than ~2 weeks, refresh it from the public changelog page.

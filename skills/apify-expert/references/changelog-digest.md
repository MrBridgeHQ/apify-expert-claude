# Apify changelog digest (manually maintained)

**Newest-first.** Curated from <https://apify.com/change-log>. Source of truth for *recent* platform changes; stable facts live in the other `references/*` files. To refresh, see the "Keeping the changelog digest current" section in `SKILL.md`.

**Format per entry:**
```
## YYYY-MM-DD - <title>  [<area tag(s)>] [→ <impacted-domain>]
<1–2 sentence digest of what changed + why it matters.>
Source: https://apify.com/change-log
```

**Impact-routing legend** - `[→ domain]` means an entry may require updating content in that domain:
`scraper-building` · `mcp-server-building` · `monetization` · `store-content` · `scraping-doctrine` · `scraper-audit` · `apify-expert` (this skill's own references). No tag = general platform info, no downstream action.

---

## ⚠ Needs human review

- 2026-06-09 - MCP connectors are live. Actors now work where you do. → review mcp-server-building (new OAuth/connector capability for Standby Actors; update architecture docs and capability overview)

---

<!-- INSERT-NEW-ENTRIES-BELOW - new changelog entries go directly under this marker, newest-first. Do not remove this marker. -->

## 2026-06-25 - New Actor creation flow  [Actor, Console] [→ scraper-building] [→ mcp-server-building]
Creating Actors on Apify now uses a guided flow that surfaces relevant templates upfront, making it faster to bootstrap new Actors from scratch.
Source: https://apify.com/change-log

## 2026-06-24 - Publish tasks for your Actor to get more users  [Actor, Store] [→ store-content]
Actor creators can now publish pre-configured tasks as dedicated Store landing pages, giving each use-case its own discoverable URL to increase Actor visibility and reach.
Source: https://apify.com/change-log

## 2026-06-09 - MCP connectors are live. Actors now work where you do.  [Console, Actor, Integrations, MCP] [→ mcp-server-building] [→ apify-expert]
Apify Actors can now securely connect to third-party apps that require login (Notion, Slack, GitHub, etc.) via **MCP connectors** - Apify manages the OAuth/credential handshake so the Actor (and the end user) never needs to handle tokens manually. This is a significant new capability for Standby/MCP Actors that call authenticated external services.
Source: https://apify.com/change-log

## 2026-05-05 - Interactive OpenAPI documentation for Standby Actors  [Actor] [→ mcp-server-building] [→ store-content]
Standby Actors that ship an OpenAPI spec now expose an **Endpoints** tab in Console to browse and test API calls in-browser. Relevant for documenting MCP/Standby Actors and for the API tab of Store listings.
Source: https://apify.com/change-log

## 2026-05-04 - Full-permission Actors now require approval  [Actor, Console] [→ mcp-server-building]
Actors requesting full account permissions now trigger a **one-time confirmation modal** before first run - a security gate aimed at AI-agent Actors. Affects onboarding UX expectations for permission-heavy Actors.
Source: https://apify.com/change-log

## 2026-04-23 - Deploy agents faster with an improved MCP configurator  [Integrations, MCP] [→ mcp-server-building]
The Apify MCP configurator was redesigned with a simpler flow to connect Apify across multiple LLM clients, speeding up agent setup.
Source: https://apify.com/change-log

## 2026-04-23 - Multiple datasets for Actors  [Actor] [→ scraper-building] [→ store-content]
Actors can now output to **multiple datasets simultaneously**, each with its own schema and validation rules - previously a run wrote to a single default dataset. Affects output-schema design and scraper architecture.
Source: https://apify.com/change-log

## 2026-04-02 - Edit Actor build tags in Apify Console  [Actor, Console]
Build tags can be added, removed, and reassigned directly in Console without rebuilding.
Source: https://apify.com/change-log

---

_Baseline established 2026-06-04 from the changelog's most recent visible entries. Entries before 2026-04-02 were not backfilled. Entries older than ~18 months should be moved to `changelog-archive.md`._

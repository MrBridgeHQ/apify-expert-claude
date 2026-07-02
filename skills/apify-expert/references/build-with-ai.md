# Building Actors with AI

Apify's official guidance for building new Actors, or improving existing ones, with AI coding
assistants (Claude Code, Cursor, GitHub Copilot, Codex, Gemini CLI). The methods below are
complementary and stack: start from a prompt or a template, then layer on Agent Skills and the
docs MCP so the assistant grounds its code in current platform facts.

> Source: <https://docs.apify.com/platform/actors/development/quick-start/build-with-ai>
> (verified 2026-07-02). Every Apify docs page also has a **Copy for LLM** button.

## Two ways to start

**From a prompt.**
1. `mkdir my-new-actor` and open it in Claude Code / Cursor / VS Code + Copilot.
2. Paste Apify's pre-built **AI coding assistant prompt** (the "Show prompt / Copy prompt" block
   on the source page). It walks the assistant through the whole loop: set up the Actor
   structure, configure all required files, install dependencies, run locally, `apify login`,
   and `apify push`.

**From a template (recommended for production).**
1. `apify create` scaffolds an Actor from an [Actor Template](https://apify.com/templates).
2. Every template ships an **`AGENTS.md`** at the project root that assistants auto-discover for
   context. No extra configuration needed. (Template catalogue and selection rationale:
   `cli-and-sdk.md`.)

## `AGENTS.md` in every template (the load-bearing fact)

`apify create` templates now include an `AGENTS.md`. Claude Code, Cursor, Codex, and Gemini CLI
read it automatically, so the assistant starts with Apify-specific conventions already in
context. Keep it and any project-level `CLAUDE.md` complementary rather than contradictory: the
`AGENTS.md` carries Apify platform conventions; your `CLAUDE.md` carries your own project rules
(source-code hygiene, PPE defaults, coding standards).

## Official Apify Agent Skills

```bash
npx skills add apify/agent-skills
```

Run inside the project directory. Installs Apify's **official** skills for Actor development,
web scraping, data extraction, and automation. They work with Claude Code, Cursor, Codex, Gemini
CLI, and other assistants, and are auto-discovered (no config). Repo:
<https://github.com/apify/agent-skills>.

Treat them as a strong baseline for generic Actor development, and layer your own project
conventions (pricing/PPE rules, deploy hygiene, anti-bot strategy) on top: those decisions are
higher-stakes and usually project-specific, so keep them authoritative where they conflict with
a generic default.

## Apify docs MCP (dev-time grounding)

A docs-scoped MCP endpoint gives the assistant tools to **search and fetch Apify documentation**
while it writes code, which sharply reduces stale/hallucinated platform facts.

- Claude Code: `claude mcp add apify "https://mcp.apify.com/?tools=docs" -t http`
- Cursor / VS Code (`mcp.json`):

  ```json
  {
    "mcpServers": {
      "apify": { "url": "https://mcp.apify.com/?tools=docs" }
    }
  }
  ```

Note the scope: `mcp.apify.com/?tools=docs` is the **documentation** surface for grounding code
generation. It is different from the full Apify MCP (`mcp.apify.com`) used to *run* Actors and
browse the Store (that access pattern lives in `api-and-integrations.md`). For Actor development,
`?tools=docs` is the one you want.

## Feeding Apify docs to the model directly

Three no-MCP ways to hand authoritative docs to any assistant (all are first-party public docs):

- **Copy for LLM** button on every docs page.
- **`.md` suffix** on any docs URL returns its Markdown source, e.g.
  `https://docs.apify.com/platform/actors.md`.
- **Whole-docs bundles** (llmstxt.org standard):
  - `https://docs.apify.com/llms.txt` - Markdown index of all pages.
  - `https://docs.apify.com/llms-full.txt` - the entire documentation in one file.

  Assistants do not auto-discover `llms.txt`; paste the link in to raise answer quality.

## Claude Code on the web / cloud sandboxes

Cloud sessions (Claude Code on the web, Codex Cloud) run in a **default-deny network sandbox**
that blocks `*.apify.com`, so the docs, REST API, and MCP server are unreachable until you allow
the domain:

1. Environment selector -> **Add environment**.
2. **Network access** -> **Custom**.
3. Check **Also include default list of common package managers**.
4. Add `*.apify.com` to **Allowed domains** (or set **Network access** to **Full**).

Not relevant to a local (desktop / WSL) Claude Code session, where outbound is unrestricted, but
it is the first thing to fix if an Actor build on the web cannot reach Apify.

## Best practices (from the guide)

- **Small tasks.** One thing at a time; break complex work into steps.
- **Iterative.** Start from a basic implementation, add complexity gradually.
- **Version often.** Commit with git frequently so you can track and roll back.
- **Security.** Never expose API keys or secrets in code or in the conversation with the
  assistant. Keep keys in a secret store, never inline.

## Related official guides

- **Develop AI agents on Apify** (building/deploying AI agents as Actors: templates, sandboxes,
  LLM access, monetization):
  <https://docs.apify.com/platform/actors/development/quick-start/develop-ai-agents>
- **Apify for AI agents** (connecting an external agent to Apify: MCP, Agent Skills, client
  libraries, REST API): <https://docs.apify.com/platform/integrations/agent-onboarding>

## How this fits Actor development

| Building... | Use, plus | Grounding |
|---|---|---|
| A scraper Actor | `apify create` template (AGENTS.md) + a scraper-building workflow | docs MCP `?tools=docs`, llms.txt |
| An MCP server Actor | `apify create --template ts-mcp-server` (AGENTS.md) + an MCP-server workflow | docs MCP `?tools=docs` |
| Any Actor | official `apify/agent-skills` as a second opinion | Copy for LLM, `.md` URLs |

The AI-assist layer accelerates scaffolding and grounds platform facts; it does **not** override
your development doctrine. PPE correctness, source-code hygiene on `apify push`, and anti-bot
strategy stay owned by their dedicated workflows.

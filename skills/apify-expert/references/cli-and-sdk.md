# Apify CLI & SDK

Baseline reference for the `apify` CLI and the JS/Python SDKs. Command surface is stable but templates and flags drift — confirm against <https://docs.apify.com/cli> and <https://docs.apify.com/sdk>.

## CLI — installation

- `npm install -g apify-cli` (Node LTS) **or** `brew install apify-cli` (macOS/Linux via Homebrew).
- Verify: `apify --version`. Authenticate: `apify login` (token from Console → Settings → Integrations).

## CLI — core commands

| Command | Purpose |
|---|---|
| `apify login` / `logout` | Authenticate the CLI with your Apify token |
| `apify create <name> [--template <id>]` | Scaffold a new Actor from a template |
| `apify init` | Turn the current dir into an Actor (adds `.actor/`) |
| `apify run [--purge] [--input-file]` | Run the Actor **locally** (`--purge` clears local storage first) |
| `apify push [--version-number] [--build-tag]` | Build & deploy the Actor to the Apify platform |
| `apify pull <actorId>` | Download an Actor's source locally |
| `apify call <actorId> [--input]` | Run an Actor **on the platform** and wait for output |
| `apify actors <ls\|info\|start\|call\|...>` | Manage Actors on the platform |
| `apify actor <get-input\|set-value\|push-data\|charge\|...>` | In-run helpers (used **inside** a running Actor) |
| `apify task <run\|...>` | Manage and run saved Actor tasks |
| `apify schedule` | Manage schedules |
| `apify datasets` / `key-value-stores` / `request-queues` | Manage storages from the CLI |
| `apify secrets <add\|rm>` | Manage secret env vars referenced from input schema |

> **Push caution:** `apify push` uploads everything not in `.gitignore`/`.dockerignore`, and the REST API exposes a published Actor's source files to authenticated users even when source is hidden in the Console UI. Keep any sensitive or internal-only files out of the deployment (list them in `.gitignore`/`.dockerignore`, or store them outside the Actor directory). Verify what shipped after a push: `apify actors info <user/slug> --json | jq '.versions[0].sourceFiles[].name'`.

## `apify create` templates (current set)

**TypeScript / Node:**
- `ts-crawlee-cheerio` — static HTML (HTTP + Cheerio)
- `ts-crawlee-playwright-chrome` — JS-rendered SPAs, moderate anti-bot
- `ts-crawlee-puppeteer-chrome` — legacy browser (prefer Playwright)
- `ts-crawlee-playwright-camoufox` — hard anti-bot targets (Camoufox stealth)
- `ts-empty` / `ts-start` — custom architecture
- `ts-mcp-server` — MCP server Actor (see an MCP-server-building workflow)

**Python:**
- `python-crawlee-beautifulsoup`, `python-crawlee-playwright`, `python-crawlee-playwright-camoufox`, `python-empty`, `python-start`

Template selection rationale (cost/anti-bot trade-offs) is owned by a scraper-building workflow.

> **AI-assisted development.** Every `apify create` template ships an `AGENTS.md` at the project
> root that Claude Code / Cursor / Codex / Gemini CLI auto-read for context. For the full
> AI-build workflow (Apify's canned assistant prompt, the official `apify/agent-skills` via
> `npx skills add apify/agent-skills`, the docs-grounding MCP `mcp.apify.com/?tools=docs`, and
> the `llms.txt` / `.md` doc-feeding tricks), see `build-with-ai.md`.

## JavaScript SDK (`apify` + `crawlee`)

- **Lifecycle:** `Actor.init()` … `Actor.exit()`, or wrap logic in `Actor.main(async () => { … })`.
- **Input:** `const input = await Actor.getInput()`.
- **Output:** `await Actor.pushData(item)` (→ default dataset); `await Actor.setValue(key, value)` (→ KVS).
- **Open storages:** `Actor.openDataset(name?)`, `Actor.openKeyValueStore(name?)`, `Actor.openRequestQueue(name?)`.
- **Proxy:** `const proxy = await Actor.createProxyConfiguration({ groups, countryCode })`.
- **Events:** `Actor.on('migrating' | 'aborting' | 'persistState', …)` — persist state for resurrection.
- **PPE billing:** `await Actor.charge({ eventName })` — see a monetization workflow.
- **Crawlee** provides `CheerioCrawler`, `PlaywrightCrawler`, `PuppeteerCrawler`, `AdaptivePlaywrightCrawler`, `RequestQueue`, `SessionPool`, `AutoscaledPool`.

## Python SDK (`apify`)

- Async-first. `async with Actor: …` or `await Actor.init()` / `await Actor.exit()`.
- `await Actor.get_input()`, `await Actor.push_data(item)`, `await Actor.set_value(key, value)`.
- `await Actor.create_proxy_configuration(...)`. Pairs with Crawlee for Python.
- Python Actors are the minority on Apify (~10% of production scrapers) — default to Node unless there's a strong reason.

# Apify platform overview

Curated baseline of platform mechanics. Updated by hand when a `changelog-digest.md` entry warrants it. Figures flagged *(verify)* should be confirmed against <https://docs.apify.com> before being quoted as hard limits - they drift.

## Actors

- An **Actor** is a serverless program (a Docker image + input/output schema + metadata) that takes JSON input and produces output. Runs on Apify's cloud, via Console / API / CLI / Scheduler / other Actors.
- **Private** Actors are for personal use; **public** ones appear in the Apify Store (free or paid).
- An Actor has **builds** (Docker images, one per version) and **runs** (executions of a build).

## Runs

- **Statuses:** `READY` → `RUNNING` → terminal `SUCCEEDED` / `FAILED` / `TIMED-OUT` / `ABORTED`. Transitional: `TIMING-OUT`, `ABORTING`.
- **Memory ↔ CPU coupling (key fact):** CPU is allocated proportionally to memory. **4096 MB = 1 full vCPU core.** So 1024 MB ≈ 0.25 core, 8192 MB ≈ 2 cores. Memory must be a power of two (128, 256, 512, 1024, 2048, 4096, 8192 MB…), capped by your account's max-memory limit.
- **Timeout:** configurable per run (set `0` for unlimited). Default *(verify)* ~3600 s on many Actors but it is Actor-defined.
- **Resurrection:** a finished/aborted/timed-out run can be *resurrected* to continue from its persisted state (same run ID, same storages). Crawlee state + `Actor.reboot()` support this.
- **Billing unit:** compute = memory-GB × runtime-hours (plus proxy, storage operations, data transfer). PPE Actors add `Actor.charge()` events on top - see a monetization workflow.

## Builds & versioning

- Versions are `major.minor` (e.g. `0.0`, `1.2`). Each version builds to a Docker image.
- **Build tags** (e.g. `latest`, `beta`) point to a specific build; runs default to the `latest` tag. Tags are now editable in Console without rebuilding (changelog 2026-04-02).

## Storage (three types)

All three are accessible via Console, REST API, CLI, and SDK. Each run gets a **default** dataset, key-value store, and request queue automatically.

### Dataset
- **Append-only, tabular** storage for results (rows of JSON). Ideal for scraped items.
- Per-item size limit **~9 MB** *(verify)*.
- Export: JSON, JSONL, CSV, Excel, XML, RSS, HTML.
- **Multiple datasets per Actor** are now supported, each with its own schema + validation (changelog 2026-04-23) - previously one default dataset per run.

### Key-value store (KVS)
- Stores **records by key**, any content type (JSON, HTML, images, binaries, text).
- Conventions: the `INPUT` record holds the Actor's input JSON; `OUTPUT` is a common output key.
- Use it for: Actor input, single-blob outputs, files, caches, screenshots, intermediate state.

### Request queue (RQ)
- Queue of URLs/requests to process; **deduplicated by `uniqueKey`**.
- Supports `forefront` (priority) adds. Crawlee crawlers use it as their frontier.

### Retention
- **Named** storages persist indefinitely.
- **Unnamed / default** storages are kept only for your plan's **data-retention period**, then auto-deleted. Name a storage (or push to a named dataset) to keep data long-term.

## Apify Proxy

- **Datacenter** - fastest, cheapest; higher block risk. Billed per IP/usage.
- **Residential** - home/office IPs, lowest block rate; **billed per GB** (verified). Use sparingly for hard targets.
- **Google SERP** - for Google SERP extraction, with country/language targeting.
- **Sessions:** a `session` id pins a sticky IP across requests; rotate by changing the session id. Country targeting via `country` param / proxy group config.
- Connect via the proxy URL (`http://<groups>,session-<id>,country-<CC>:<password>@proxy.apify.com:8000`) or `Actor.createProxyConfiguration()` in the SDK.
- Anti-bot strategy (when/which proxy, fingerprinting) is owned by a scraping-doctrine workflow - this is just the platform mechanism.

## Schedules

- Cron-based triggers that start an Actor or task on a recurring schedule (UTC). Managed in Console or via API/CLI.

## Webhooks

- Fire on run lifecycle events (`ACTOR.RUN.SUCCEEDED`, `FAILED`, `CREATED`, etc.) to an HTTP endpoint. Used to chain Actors, notify, or post-process. Payload templating supported.

## Integrations & monitoring

- **Native integrations:** Zapier, Make, n8n, Slack, GitHub, Google Drive/Sheets, LangChain, LlamaIndex, vector DBs (Pinecone, Qdrant), and the **Apify MCP server** (see `references/api-and-integrations.md`).
- **Monitoring:** run metrics (CPU, memory, compute units), logs, and the Monitoring suite for alerting on failures/empty datasets.

## Cross-references

- Scraper Actor code/architecture → a scraper-building workflow
- MCP server Actor code → an MCP-server-building workflow
- Pricing/PPE/billing → a monetization workflow
- README/schemas/Store content → a Store-content authoring workflow
- Anti-bot/proxy doctrine → a scraping-doctrine workflow

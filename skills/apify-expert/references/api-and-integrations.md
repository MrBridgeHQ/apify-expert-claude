# Apify REST API & integrations

Baseline for the Apify REST API, authentication, and the Apify MCP access pattern. Confirm endpoints against <https://docs.apify.com/api/v2>.

## REST API

- **Base URL:** `https://api.apify.com/v2`.
- **Auth:** token via header `Authorization: Bearer <APIFY_TOKEN>` (preferred) or `?token=<APIFY_TOKEN>` query param. Tokens are created in Console → Settings → Integrations.

### Key endpoint families

| Family | Example | Purpose |
|---|---|---|
| Actors | `GET /v2/acts/{actorId}` | Actor metadata, builds, versions |
| Run an Actor | `POST /v2/acts/{actorId}/runs` | Start a run (async; returns run object) |
| Run sync | `POST /v2/acts/{actorId}/run-sync-get-dataset-items` | Run and get dataset items in one blocking call |
| Runs | `GET /v2/actor-runs/{runId}` | Run status/metadata; `?waitForFinish=` to block |
| Datasets | `GET /v2/datasets/{id}/items` | Read items (supports `offset`/`limit` pagination, `format`, `fields`, `clean`) |
| Key-value stores | `GET /v2/key-value-stores/{id}/records/{key}` | Read a record |
| Request queues | `/v2/request-queues/{id}/requests` | Enqueue / read requests |
| Schedules | `/v2/schedules` | CRUD schedules |
| Webhooks | `/v2/webhooks` | CRUD webhooks |
| Tasks | `/v2/actor-tasks` | Saved Actor configurations |
| Logs | `GET /v2/logs/{buildOrRunId}` | Stream run/build logs |

- **Pagination:** dataset items via `offset` + `limit` (default page size *(verify)*; large datasets must be paginated). Use `fields` to project columns, `clean=true` to drop empty/hidden items.
- **`isSourceCodeHidden`** hides source in the Console UI **but the API still exposes `versions[0].sourceFiles[]`** to authenticated users — keep sensitive/internal files out of the deployment entirely rather than relying on this flag.

## Apify MCP server (for AI agents)

Two access patterns:

1. **Hosted Apify MCP** — `https://mcp.apify.com`. A single MCP endpoint that exposes Store-Actor search + execution as tools. Auth with an Apify token. Lets an AI client discover and run any Store Actor. The **MCP configurator** (redesigned 2026-04-23) generates per-client setup snippets.
2. **Per-Actor MCP (Standby)** — an MCP server Actor exposes its own tools at `https://<user>--<actor-name>.apify.actor/mcp` while running in Standby. Building these is owned by an MCP-server-building workflow.

- Transports: stdio (local), SSE (legacy), **Streamable HTTP** (current default for hosted/Standby MCP).
- Auth to a Standby MCP: Bearer token in `Authorization`, or `?token=` query param.

## Native integrations

- **Automation platforms:** Zapier, Make, n8n.
- **Notification/data sinks:** Slack, Google Drive/Sheets, GitHub, email, generic webhooks.
- **AI/RAG:** LangChain, LlamaIndex, vector-DB integrations (Pinecone, Qdrant, …), and the Apify MCP server.
- **Webhooks** are the lowest-level integration primitive — chain Actors or trigger external systems on run events.

## Cross-references

- MCP server **code & architecture** → an MCP-server-building workflow
- PPE billing over the API/Standby → a monetization workflow

# Qoory Operations

Use this reference only when choosing a Qoory MCP tool or REST fallback. Check the credit cost before making optional follow-up calls. For REST request fields and bounds, use `requests.md`.

## Search First

| Need | Required scope | Credit cost | MCP tool | REST fallback |
|---|---|---|---|---|
| General entity search | `search:read` | 1 credit | `search_entities` | `POST /v1/search` |
| Resolve a name to ids/paths | `search:read` | 1 credit | `resolve_entities` | `GET /v1/entities/resolve` |
| People-only lookup | `search:read` | 1 credit | `search_people` | `GET /v1/people/search` |

Reject bulk-style prompts such as "list all projects" or "export every person". Ask for a narrower query or use a curated discovery endpoint.

Search results are strict identity/reference projections: they do not include profiles, media, social fields, relationships, market data, ranking signals, or per-type counts. Use a selected result's canonical Qoory id or slug with the narrow one-record detail tool. X-account results use opaque Qoory UUIDs and never echo handles.

## Entity Details

| Need | Required scope | Credit cost | MCP tool | REST fallback |
|---|---|---|---|---|
| Qoory project identity | `data:read` | 1 credit | `get_project` | `GET /v1/projects/{slug}` |
| Fund identity and website | `data:read` | 1 credit | `get_fund` | `GET /v1/funds/{slug}` |
| Person identity | `data:read` | 1 credit | `get_person` | `GET /v1/people/{slug}` |
| X account identity and link | `data:read` | 1 credit | `get_x_account` | `GET /v1/x-accounts/{id}` |
| Token identity | `data:read` | 1 credit | `get_token` | `GET /v1/tokens/{id}` |
| Stock profile | `data:read` | 1 credit | `get_stock` | `GET /v1/stocks/{slug}` |
| Event detail | `data:read` | 1 credit | `get_event` | `GET /v1/events/{slug}` |
| News detail | `data:read` | 1 credit | `get_news` | `GET /v1/news/{id}` |

## Relationships And Evidence

| Need | Required scope | Credit cost | MCP tool | REST fallback |
|---|---|---|---|---|
| Project funding | `data:read` | 2 credits | `get_project_funding` | `GET /v1/projects/{slug}/funding` |
| Project market | `data:read` | 2 credits | `get_project_market` | `GET /v1/projects/{slug}/market` |
| Project timeline | `data:read` | 2 credits | `get_project_events` | `GET /v1/projects/{slug}/events` |
| Project news | `data:read` | 2 credits | `get_project_news` | `GET /v1/projects/{slug}/news` |
| Project linked tokens | `data:read` | 1 credit | `get_project_tokens` | `GET /v1/projects/{slug}/tokens` |
| Project TVL | `data:read` | 2 credits | `get_project_tvl` | `GET /v1/projects/{slug}/tvl` |
| Project features | `data:read` | 4 credits | `get_project_features` | `GET /v1/projects/{slug}/features` |
| Fund portfolio | `data:read` | 2 credits | `get_fund_portfolio` | `GET /v1/funds/{slug}/portfolio` |
| Fund partners | `data:read` | 2 credits | `get_fund_partners` | `GET /v1/funds/{slug}/partners` |
| Fund news | `data:read` | 2 credits | `get_fund_news` | `GET /v1/funds/{slug}/news` |
| Person roles | `data:read` | 2 credits | `get_person_roles` | `GET /v1/people/{slug}/roles` |
| Person investments | `data:read` | 2 credits | `get_person_investments` | `GET /v1/people/{slug}/investments` |
| Person news | `data:read` | 2 credits | `get_person_news` | `GET /v1/people/{slug}/news` |
| Stock news | `data:read` | 2 credits | `get_stock_news` | `GET /v1/stocks/{slug}/news` |

`get_project_tvl` returns HTTP 200 with `availability: "unavailable"`, null current fields, and an empty history when the project exists but has no eligible current TVL series. Treat that as a terminal valid result: report that TVL is unavailable and do not retry or reinterpret it as a missing project.

## Markets, Discovery, And Narratives

| Need | Required scope | Credit cost | MCP tool | REST fallback |
|---|---|---|---|---|
| Trending projects | `data:read` | 2 credits | `list_trending_projects` | `GET /v1/projects/trending` |
| Trending topics | `data:read` | 2 credits | `list_trending_topics` | `GET /v1/discover/trending` |
| Recent news | `data:read` | 2 credits | `list_recent_news` | `GET /v1/news/recent` |
| Upcoming events | `data:read` | 2 credits | `list_upcoming_events` | `GET /v1/events/upcoming` |
| Narrative list | `data:read` | 2 credits | `list_narratives` | `GET /v1/narratives/discover` |
| Narrative detail | `data:read` | 2 credits | `get_narrative` | `GET /v1/narratives/{slug}` |
| Narrative timeline | `data:read` | 2 credits | `get_narrative_timeline` | `GET /v1/narratives/{slug}/timeline` |
| Narrative evidence | `data:read` | 2 credits | `get_narrative_evidence` | `GET /v1/narratives/{slug}/evidence` |
| Token trending | `data:read` | 2 credits | `list_trending_tokens` | `GET /v1/tokens/trending` |
| Token market | `data:read` | 2 credits | `get_token_market` | `GET /v1/tokens/{id}/market` |
| Tokenomics | `data:read` | 2 credits | `get_tokenomics` | `GET /v1/tokenomics/{id}` |
| Token unlocks | `data:read` | 2 credits | `get_token_unlocks` | `GET /v1/tokens/{id}/unlocks` |
| Market top movers | `data:read` | 1 credit | `list_market_top_movers` | `GET /v1/markets/top-movers` |
| Market snapshot | `data:read` | 1 credit | `get_market_snapshot` | `GET /v1/markets/snapshot` |
| Crypto heatmap | `data:read` | 2 credits | `get_crypto_heatmap` | `GET /v1/markets/crypto-heatmap` |
| Stock rankings | `data:read` | 2 credits | `list_stock_rankings` | `GET /v1/stocks/rankings` |
| Fund rankings | `data:read` | 2 credits | `list_fund_rankings` | `GET /v1/funds/rankings` |

## Response Handling

- `MISSING_API_KEY`, `INVALID_API_KEY`, `DISABLED_API_KEY`, or `REVOKED_API_KEY`: stop. Do not retry or ask the user to paste a key into chat; direct them to configure or replace the key in their client secret store.
- `USER_SUSPENDED`, `FEATURE_DISABLED`, `CONFIGURATION_ERROR`, or `FORBIDDEN`: stop. Do not retry authentication, authorization, account, feature, or configuration failures.
- `RATE_LIMITED`: report the retry/rate-limit metadata if present and stop.
- `INSUFFICIENT_CREDITS`: tell the user the key lacks credits.
- `NOT_FOUND`: ask for a more specific slug/id or run search.
- `INVALID_REQUEST`: narrow the query, reduce `limit`, or fix filters.
- `DATA_UNAVAILABLE`: report that current verified data is unavailable and stop. Do not retry automatically, use stale data, or infer a replacement value.
- `INTERNAL_ERROR`: report the bounded request id when present and stop. Do not automatically retry or expose underlying diagnostics.
- Unknown error codes: stop and report the bounded public error without guessing a workaround.

# Common n8n Workflow Patterns

Use these as starting architectures when designing offline workflows.

## 1) Webhook intake → validate/normalize → route → action
**Use when:** External systems push event payloads.

Typical nodes:
1. `Webhook`
2. `Set` or `Code` (normalize payload)
3. `IF` / `Switch` (route by event type or quality)
4. Destination action nodes (CRM, ticketing, notifications)

Notes:
- Webhook data is often under `$json.body`.
- Include a reject/error branch for invalid payloads.

## 2) Scheduled sync → fetch → transform → upsert
**Use when:** Pulling periodic updates from an API/source.

Typical nodes:
1. `Schedule Trigger`
2. Source fetch node (or HTTP Request)
3. `Set` / `Code` transform
4. Upsert/update destination node

Notes:
- Add idempotency key strategy (external ID, composite key).
- Mention pagination/rate-limit handling as TODO if unknown.

## 3) Batch ingest → per-item process → aggregate/report
**Use when:** Processing lists and summarizing outcomes.

Typical nodes:
1. Trigger/fetch
2. Item transform/validation
3. Action node per item
4. JS `Code` aggregate summary
5. Notification/report node

Notes:
- Define success/failure counters and output report shape.

## 4) Resilient path with retry/fallback
**Use when:** Integrations are flaky or rate-limited.

Typical nodes:
1. Trigger/fetch
2. Primary action node
3. Error branch (`IF` / dedicated failure routing)
4. `Wait` + retry attempt or fallback notification/sink

Notes:
- Make retry behavior explicit and bounded.
- Keep dead-letter/failure output auditable.

## Selection heuristic
- If source pushes events: start with Webhook pattern.
- If source must be polled: start with Scheduled sync.
- If user asks for summary output: include Batch + aggregate.
- If reliability is emphasized: add Retry/Fallback pattern elements.

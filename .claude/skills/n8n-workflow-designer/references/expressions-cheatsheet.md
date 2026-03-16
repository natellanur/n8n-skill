# n8n Expressions Cheatsheet

Use expressions for lightweight mapping and defaults.

## Common data access
- Current item object: `{{ $json }}`
- Current field: `{{ $json.email }}`
- Nested field: `{{ $json.body.customer.id }}`
- Previous node by name: `{{ $node["Normalize"].json.customerId }}`
- Workflow static value example: `{{ $workflow.name }}`
- Execution timestamp: `{{ $now }}`

## Safe fallback patterns
- Default string: `{{ $json.firstName || "Unknown" }}`
- Default number: `{{ Number($json.amount || 0) }}`
- Null-safe nested read: `{{ $json.body?.company?.name || "" }}`
- Normalize email: `{{ String($json.email || "").trim().toLowerCase() }}`

## Boolean and conditional patterns
- Simple conditional: `{{ $json.score > 80 ? "high" : "normal" }}`
- Presence check: `{{ !!$json.phone }}`
- Multi-condition flag: `{{ $json.country === "US" && $json.optIn === true }}`

## Date and formatting patterns
- ISO now: `{{ $now.toISO() }}`
- Date plus offset (example): `{{ $now.plus({ days: 1 }).toISODate() }}`
- Join array: `{{ ($json.tags || []).join(",") }}`

## Practical guidance
- Prefer expressions in `Set`, filter conditions, and straightforward field maps.
- If expressions become long or repeated, move logic into a JS Code node.
- Validate that each expression references fields created upstream.

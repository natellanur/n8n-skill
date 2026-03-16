---
name: n8n-workflow-designer
description: Consolidated offline n8n design skill inspired by czlonkowski/n8n-skills; guides workflow architecture, node setup, expressions, validation, and Code node output without MCP or live n8n access.
---

# n8n-workflow-designer

## Description
This skill is a **single offline consolidation** of the multi-skill design in `czlonkowski/n8n-skills`.

Repository-inspired domain coverage:
1. n8n Expression Syntax
2. n8n MCP Tools Expert (**MCP behavior removed here**)
3. n8n Workflow Patterns
4. n8n Validation Expert
5. n8n Node Configuration
6. n8n Code JavaScript
7. n8n Code Python

This skill consolidates those domains into one implementation-focused workflow designer, explicitly constrained to **offline workflow design + JSON generation for manual import**.

## When to use
Use when the user needs:
- End-to-end n8n workflow design from a business goal.
- Node selection + operation-aware node configuration guidance.
- Expression design and field mapping.
- JavaScript or Python snippets for Code nodes.
- Validation thinking before delivering workflow JSON.
- Manual setup instructions for import, credentials, triggers, and testing.

## When not to use
Do **not** use for:
- MCP tooling or MCP workflow execution.
- Any live n8n inspection or runtime interaction.
- Workspace inspection or live environment introspection as a required workflow step.
- Credential retrieval/inspection/generation.
- Claims about deployment success, run success, or production verification.

## Design priority
Default preference order:
1. Native n8n nodes and expressions.
2. `IF` / `Switch` / `Merge` for routing and branching.
3. JavaScript Code node only when native nodes become too complex.
4. Python Code node only when explicitly requested.

## Core behavior
1. Convert user intent into a workflow pattern (trigger + transform + branch + side effects).
2. Choose nodes and operations that match the target system behavior.
3. Define reliable connections and data contracts between nodes.
4. Prefer expressions for straightforward mapping; use Code nodes when logic is branching/iterative/complex.
5. Generate import-ready n8n JSON scaffold with clear placeholders.
6. Run offline validation checks before returning output.
7. Provide explicit manual setup and troubleshooting actions.
8. Mark unknowns as assumptions/TODOs; never fabricate environment facts.

## Workflow design methodology
### 1) Pattern selection
Pick one primary pattern and state it:
- Webhook intake → validate/normalize → route → external action.
- Schedule poll → transform/aggregate → notify/writeback.
- Event processing with retry/backoff and dead-letter path.
- Batch ingest + per-item loop + error branch.

### 2) Connection best practices
- Ensure each non-trigger node has an upstream path.
- Keep happy-path and error-path visually distinct.
- Use explicit branch nodes (`IF`, `Switch`) for decision points.
- Keep merge strategy explicit (`Merge` mode and expected item shape).

### 3) Operation-aware node setup
- Specify operation per node (e.g., create/update/getMany).
- Define required fields vs optional fields.
- Document idempotency strategy (dedupe key, upsert behavior).
- Include pagination/rate-limit behavior when applicable.

### 4) Pre-JSON validation thinking
Before returning JSON, check:
- Is trigger configuration coherent with requested behavior?
- Are data fields available where expressions reference them?
- Are branch outcomes connected to terminal actions?
- Are retries/failure handling represented or intentionally omitted?

## Expression Syntax Guidance
- Default to n8n expressions for simple mappings and value derivation.
- Common references:
  - Current item: `{{ $json }}` / `{{ $json.field }}`
  - Webhook payload often lives under: `{{ $json.body }}`
  - Upstream node data by name: `{{ $node["Node Name"].json.field }}`
- Use expressions when:
  - Mapping fields between nodes.
  - Simple formatting and conditional defaults.
  - Lightweight computed values.
- Avoid expression overuse when logic becomes multi-step or hard to read; move to Code node.

## Workflow Patterns Guidance
- **Webhook processing**: `Webhook` → `Set/Code` normalization → `IF/Switch` route → API/action nodes.
- **Scheduled sync**: `Schedule Trigger` → source fetch → transform → upsert/write.
- **Notification pipeline**: trigger/source → filter/priority split → channel-specific send nodes.
- **Resilient integration**: source fetch → guarded operation → retry/wait branch + failure sink.

For each pattern, specify:
- Trigger contract (expected input shape).
- Processing contract (what fields are created/changed).
- Output contract (what terminal nodes require).

## Validation Guidance
Perform explicit offline validation in the response:
- **Structural**: nodes exist, named consistently, and are connected.
- **Data-path**: expressions reference fields created upstream.
- **Operational**: node operations match intent (create vs update vs upsert).
- **Failure modes**: identify likely break points and mitigation.

Common failure points to call out:
- Wrong payload path (e.g., expecting root fields when data is in `$json.body`).
- Null/undefined fields in expressions.
- Merge/branch producing unexpected item arrays.
- API nodes missing required mapped fields.

## Node Configuration Guidance
- Always provide node-by-node configuration highlights:
  - Node purpose.
  - Operation.
  - Key parameters.
  - Required input fields.
  - Produced output fields.
- Use placeholders for environment-specific values (URLs, IDs, credential references).
- Distinguish mandatory manual edits from optional tuning.

## Code Node JavaScript Guidance
Use JavaScript Code nodes when logic is too complex for expressions.

Practical n8n patterns:
- Access all input items: `$input.all()`
- Access first input item: `$input.first()`
- Access current item context: `$input.item`

Return shape expectations:
- Return an array of items with `json` payload, e.g.:
```javascript
return [{ json: { result: "ok" } }];
```
- For mapped multi-item transforms:
```javascript
const out = $input.all().map(item => ({
  json: {
    ...item.json,
    normalizedEmail: String(item.json.email || '').trim().toLowerCase(),
  },
}));

return out;
```

## Code Node Python Guidance
Python Code nodes are supported, but **JavaScript is the default recommendation** in most n8n workflows due to ecosystem examples, maintainability, and team familiarity.

Use Python only when the user explicitly requests it or has strong Python-specific constraints.
- Keep Python snippets minimal and deterministic.
- Document expected input/output item structure clearly.
- Ensure output shape still matches n8n item expectations (`json` payload per item).

## JSON generation rules
- Prefer minimal valid structure over speculative, detailed configuration.
- Use clear placeholders for credentials, IDs, URLs, and secrets.
- Keep node names simple and stable.
- Ensure connections reflect the described workflow logic exactly.
- If exact parameters are uncertain, use `TODO:` placeholders instead of inventing values.

## Output format
Always respond in this exact section order:
1. **Goal**
2. **Workflow logic**
3. **Nodes**
4. **Connections**
5. **Expressions / code**
6. **n8n JSON**
7. **Manual setup required**

## Validation rules
Before finalizing output, enforce all checks:
- Workflow matches stated goal and selected pattern.
- Node operations are explicitly defined where relevant.
- Non-trigger nodes are connected from valid upstream paths.
- Expressions reference plausible data locations.
- Code snippets are syntactically coherent and return n8n-compatible items.
- JSON scaffold includes nodes, connections, and editable placeholders.
- Unknown environment facts are marked as assumptions/TODO.
- No claim of live testing, deployment, or credential verification.

## Response style
- Be concrete, n8n-specific, and implementation-first.
- Favor short, actionable node guidance over generic architecture prose.
- Include troubleshooting notes where failure risk is high.
- Use direct language and avoid false confidence.

## Troubleshooting behavior
When uncertainty exists:
- State assumptions explicitly.
- Add `TODO:` placeholders for unknown IDs, URLs, credentials, or schema details.
- Highlight what must be manually validated in n8n after import.
- Never imply that runtime behavior was confirmed.

## Post-import verification checklist
After import, manually verify:
- Trigger configuration (path, schedule, event filters, test mode).
- Credentials attachment to each node requiring auth.
- Node operation choice (create/update/upsert/getMany/etc.).
- Expression field paths (especially nested paths like `$json.body`).
- Expected item shape after each transform/Code step.
- Branch and error-path behavior, including retry/fallback handling.

## Examples
### Example A — Webhook to CRM
- Recognize payload likely at `$json.body`.
- Normalize in `Set` or JS `Code` node.
- Map to CRM create/update operation.
- Include manual checklist for webhook test payload + credentials.

### Example B — Daily aggregation email
- `Schedule Trigger` + fetch node + JS aggregation.
- Use `$input.all()` for grouping.
- Return summary items and map into email body.

### Example C — Retry-aware API workflow
- Add route for transient failures and retry delay.
- Document retry caps and dead-letter/manual review path.
- Mark backoff parameters as TODO placeholders if unknown.

## Boundaries
This skill must explicitly avoid:
- MCP usage and MCP-dependent assumptions.
- Live n8n access or workspace inspection.
- Credential access or secret inference.
- Deployment/runtime verification claims.

If required details are missing, request them or proceed with clearly marked placeholders.

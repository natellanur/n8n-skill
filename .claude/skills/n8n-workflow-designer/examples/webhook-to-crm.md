# Example Task: Webhook to CRM

## User request (example)
"Design an n8n workflow that receives lead webhooks and creates or updates contacts in our CRM. Return import JSON."

## Expected skill response style
1. State goal and selected pattern: **Webhook intake → normalize → route → CRM upsert**.
2. Propose nodes: `Webhook`, `Set/Code`, `IF` (valid email?), CRM node, failure notification/log.
3. Include key expressions such as:
   - `{{ $json.body.email }}`
   - `{{ String($json.body.email || '').trim().toLowerCase() }}`
4. Provide offline JSON scaffold with placeholders:
   - `TODO: CRM_CREDENTIAL_NAME`
   - `TODO: CRM_INSTANCE_URL`
5. End with manual setup checklist (credentials, webhook test payload, field mapping verification).

## Constraints reminder
- Do not claim connection to real CRM or live n8n testing.
- Keep output import-focused and editable.

# Example Task: Daily Digest

## User request (example)
"Create a daily digest workflow that pulls new tickets, summarizes counts by priority, and emails a report."

## Expected skill response style
1. State goal and selected pattern: **Scheduled sync → fetch → aggregate → notify**.
2. Propose nodes: `Schedule Trigger`, fetch node/HTTP Request, JS `Code` summary, email node.
3. Show JS aggregation example using `$input.all()` and returning:
   - `[{ json: { total, high, medium, low } }]`
4. Provide import JSON scaffold with placeholders:
   - `TODO: TICKET_API_URL`
   - `TODO: EMAIL_CREDENTIAL_NAME`
   - `TODO: RECIPIENT_LIST`
5. Include manual checks for schedule timing, API auth, and output field mapping into email template.

## Constraints reminder
- Offline design only.
- No runtime validation claims.

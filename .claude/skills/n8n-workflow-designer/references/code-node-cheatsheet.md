# n8n Code Node Cheatsheet (JavaScript-first)

Use JS Code node when expressions become hard to maintain.

## Default guidance
- Prefer JavaScript unless Python is explicitly requested.
- Keep transforms deterministic and side-effect free.
- Return n8n item arrays with a `json` object per item.

## `$input` patterns
- All items:
```javascript
const items = $input.all();
```

- First item:
```javascript
const first = $input.first();
```

- Current item context:
```javascript
const item = $input.item;
```

## Return-shape examples
- Single item:
```javascript
return [{ json: { ok: true } }];
```

- Map items:
```javascript
return $input.all().map((item) => ({
  json: {
    ...item.json,
    emailNormalized: String(item.json.email || '').trim().toLowerCase(),
  },
}));
```

- Filter + map:
```javascript
return $input
  .all()
  .filter((item) => item.json.active)
  .map((item) => ({ json: { id: item.json.id, status: 'active' } }));
```

- Aggregate summary:
```javascript
const items = $input.all();
const success = items.filter(i => i.json.success === true).length;
const failed = items.length - success;

return [{ json: { total: items.length, success, failed } }];
```

## Guardrails
- Avoid mutating shared state across executions.
- Handle missing fields with defaults.
- Keep large business rules documented in comments.
- If output contract is unclear, emit explicit fields and note TODOs.

---
name: n8n-workflow-designer
description: Offline n8n workflow design skill focused on architecture, expressions, Code node guidance, and import-ready JSON generation for manual use.
---

# n8n-workflow-designer

## Description
This skill designs n8n workflows **offline** and generates JSON for **manual import**.

Use this skill to:
- Translate business requirements into practical node-by-node workflow designs.
- Map data with expressions and lightweight transforms.
- Add JavaScript Code node logic when expressions are no longer readable.
- Return import-ready JSON scaffolds with safe placeholders.

Support files (use as needed):
- `references/expressions-cheatsheet.md`
- `references/n8n-patterns.md`
- `references/code-node-cheatsheet.md`
- `examples/webhook-to-crm.md`
- `examples/daily-digest.md`

## Hard constraints
- Offline workflow design and JSON generation only.
- No MCP.
- No live n8n access or runtime inspection.
- No credential retrieval or validation.
- Never claim deployment/runtime success.

## Design priority
1. Native n8n nodes and expressions.
2. `IF` / `Switch` / `Merge` for branching.
3. JavaScript Code node when logic becomes multi-step.
4. Python Code node only when explicitly requested.

## Core behavior
1. Convert intent into a workflow pattern (trigger → processing → action).
2. Choose nodes/operations that match the stated goal.
3. Define data contracts between steps (required fields in/out).
4. Use expressions for simple mapping; use Code for complex transforms.
5. Generate import-ready JSON scaffold with placeholders.
6. Provide manual setup and post-import checks.
7. Mark unknowns as `TODO:` assumptions.

## Output format
Always respond in this order:
1. **Goal**
2. **Workflow logic**
3. **Nodes**
4. **Connections**
5. **Expressions / code**
6. **n8n JSON**
7. **Manual setup required**

## Validation checklist
Before finalizing:
- Workflow matches goal and chosen pattern.
- Node operations are explicit where relevant.
- Non-trigger nodes are connected from valid upstream paths.
- Expressions reference plausible upstream fields.
- Code returns n8n-compatible item arrays (`[{ json: ... }]`).
- JSON scaffold includes editable placeholders for unknowns.
- Assumptions are explicit; no live-testing claims.

## Troubleshooting behavior
- State assumptions clearly.
- Use `TODO:` placeholders for IDs, URLs, credentials, schema unknowns.
- Highlight likely failure points (path mismatches, null fields, branch shape issues).
- Tell the user exactly what to verify manually after import.

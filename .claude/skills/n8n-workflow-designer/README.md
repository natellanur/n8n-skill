# n8n-workflow-designer

## Purpose
`n8n-workflow-designer` is a Claude skill for **offline n8n workflow design** and **manual JSON generation**.

It helps produce:
- Workflow architecture from a plain-language requirement.
- Node-by-node configuration guidance.
- Expressions and JavaScript Code node snippets.
- Import-ready JSON scaffolds with placeholders.

## Location
Place this skill at:

`.claude/skills/n8n-workflow-designer/`

## File layout
- `SKILL.md` (primary required instruction file)
- `references/expressions-cheatsheet.md`
- `references/n8n-patterns.md`
- `references/code-node-cheatsheet.md`
- `examples/webhook-to-crm.md`
- `examples/daily-digest.md`

## Usage
Prompt examples:
- "Use n8n-workflow-designer to design a webhook-to-CRM workflow and produce import JSON."
- "Use n8n-workflow-designer for a daily digest workflow with JS aggregation."

## Boundaries
- Offline only.
- No MCP usage.
- No live n8n access.
- No credential inspection.
- No claims that workflows were deployed or runtime-validated.

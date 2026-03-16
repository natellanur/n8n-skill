# n8n-workflow-designer

## What this skill is for
`n8n-workflow-designer` is an offline Claude Code skill for designing n8n workflows and generating JSON for **manual import**.

It is inspired by the domain structure of `czlonkowski/n8n-skills` and consolidated into one offline skill:
- Expression Syntax
- Workflow Patterns
- Validation
- Node Configuration
- Code JavaScript
- Code Python
- (MCP domain removed operationally)

## Where to place it
Place it at:

`.claude/skills/n8n-workflow-designer/`

Files:
- `SKILL.md`
- `README.md`

## How to call it
- “Use `n8n-workflow-designer` to design a webhook-to-CRM flow and generate import JSON.”
- “Use `n8n-workflow-designer` for a scheduled digest workflow with JS Code node aggregation.”

## Design principles
- Use native n8n nodes first.
- Prefer expressions before Code nodes when possible.
- Prefer JavaScript over Python by default.
- Use placeholders instead of invented environment values.
- Manually verify workflow behavior after import.

## Important constraints
- Offline only
- No MCP
- No live n8n access
- No credential access
- No workspace inspection requirement
- No deployment/runtime success claims

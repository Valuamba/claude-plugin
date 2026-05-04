---
name: plugin-grow
description: >
  Analyze the current state of a Claude Code plugin and suggest the next
  development milestone. Provides a concrete list of files to add or improve
  based on the growth roadmap from v0.1 to v1.0.
argument-hint: "[path]"
user-invocable: true
---

# Plugin Growth Advisor

Analyze the plugin at $ARGUMENTS (or current directory) and suggest next steps.

## Workflow

1. Read the plugin directory and identify what exists.
2. Determine the current version stage.
3. Read `references/growth-roadmap.md` for milestone definitions.
4. Suggest the next concrete milestone with specific files to create.

## Version Detection

| Has | Version |
|---|---|
| manifest + 1 skill | v0.1 |
| router skill + sub-skills + references | v0.2 |
| multiple agents + real scripts + tests | v0.3 |
| hooks + report generation + user config | v0.4 |
| MCP + API cost guards + rate limits | v0.5 |
| marketplace metadata + changelog + security + docs | v1.0 |

## Output

Return:

1. Current version stage
2. What exists (component inventory)
3. What to add next (specific files with descriptions)
4. Priority order for the additions
5. Offer to generate the files

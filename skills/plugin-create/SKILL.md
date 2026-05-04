---
name: plugin-create
description: >
  Main router for Claude Code plugin creation workflows.
  Use when the user wants to create a new plugin, scaffold plugin structure,
  audit an existing plugin, or get guidance on plugin architecture
  (skills, agents, scripts, hooks, MCP, references).
argument-hint: "[command] [target]"
user-invocable: true
---

# Plugin Creator

You are the router for the plugin creation toolkit.

## Commands

- `scaffold <name> [--minimal]` — generate a new plugin directory with all components
- `audit [path]` — audit an existing plugin for completeness and best practices
- `grow [path]` — suggest next steps to expand a plugin from its current version
- (no args) — interactive guide to help the user decide what to build

## Routing

### scaffold

Load the `plugin-scaffold` skill. Use `references/plugin-architecture.md` for the full structural guide. Generate all files following the recommended structure.

### audit

Load the `plugin-audit` skill. Delegate structural review to the `structure-reviewer` agent. Check manifest, skills, agents, scripts, references, hooks against the architecture reference.

### grow

Load the `plugin-grow` skill. Analyze current plugin version and suggest the next milestone from `references/growth-roadmap.md`.

### No arguments

Ask the user:

1. What domain is the plugin for?
2. What workflows should it support?
3. Does it need external APIs (MCP)?
4. Does it need deterministic scripts?

Then recommend a starting structure and offer to scaffold it.

## Core Principle

```
Skill = what should be done
Agent = who should do it
Script = how to check or calculate something exactly
Reference = long-term knowledge
Hook = automatic safety/quality rule
MCP = external service/tool connection
```

## Output

Always provide:

1. Clear file tree of what was created or needs to change
2. Explanation of each component's role
3. Next steps the user should take

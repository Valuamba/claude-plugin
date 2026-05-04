---
name: plugin-scaffold
description: >
  Scaffold a new Claude Code plugin with all required components.
  Generates plugin.json, marketplace.json, skills, agents, scripts, references, hooks, and README.
  Use when the user wants to create a new plugin from scratch.
argument-hint: "<plugin-name> [--minimal]"
user-invocable: true
---

# Plugin Scaffold

Generate a new plugin named $ARGUMENTS.

## Before starting

Read `references/templates.md` for file templates.
Read `references/plugin-architecture.md` (in the plugin root `references/` directory) for the full structural guide.

## Workflow

1. Parse the plugin name from arguments. If `--minimal` is present, generate MVP only.
2. Ask the user what domain the plugin targets (if not obvious from the name).
3. Generate the directory structure.
4. Create all files with meaningful content tailored to the domain.
5. Show the final file tree and explain each component.

## Full scaffold structure

```
<plugin-name>/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   ├── main/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── routing.md
│   └── <workflow>/
│       ├── SKILL.md
│       └── references/
│           └── checklist.md
├── agents/
│   └── reviewer.md
├── scripts/
│   └── collect.py
├── hooks/
│   ├── hooks.json
│   └── validate-output.py
├── references/
│   └── quality-gates.md
├── requirements.txt
└── README.md
```

## Minimal scaffold (--minimal)

```
<plugin-name>/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── main/
│       └── SKILL.md
├── agents/
│   └── reviewer.md
└── README.md
```

## Rules

- Plugin name must be kebab-case, no spaces.
- All skills must have strong `description` fields — Claude selects skills based on descriptions.
- Agents must have narrow, specific roles — not "general expert".
- Scripts must accept CLI args, return JSON, validate input, use timeouts.
- Never hardcode secrets. Use `userConfig` with `sensitive: true`.
- All paths in hooks and MCP configs must use `${CLAUDE_PLUGIN_ROOT}`.

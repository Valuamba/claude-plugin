# plugin-create

Claude Code plugin for creating Claude Code plugins. Scaffolds new plugins, audits existing structure, and provides architecture guidance for skills, agents, scripts, hooks, MCP, and references.

## Installation

```bash
# Add marketplace (one-time)
/plugin marketplace add Valuamba/claude-plugin

# Install plugin
/plugin install plugin-create@valuamba-claude-plugin
```

Or for local development/testing:

```bash
claude --plugin-dir /Users/valuamba/projs/skills/claude-plugin
```

## Commands

```
/plugin-create                            — interactive guide to help you decide what to build
/plugin-create scaffold <name>            — generate a new plugin with all components
/plugin-create scaffold <name> --minimal  — generate MVP (manifest + 1 skill + 1 agent)
/plugin-create create skill <name>        — add a new skill to an existing plugin
/plugin-create audit [path]               — audit an existing plugin for completeness and best practices
/plugin-create grow [path]                — suggest next development milestone
```

## Plugin Structure

```
claude-plugin/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   ├── plugin-create/          — main router skill
│   ├── plugin-scaffold/        — generates new plugins from templates
│   ├── create-skill/           — adds a new skill to an existing plugin
│   ├── plugin-audit/           — audits existing plugins (manifest, skills, agents, security)
│   └── plugin-grow/            — suggests next milestone (v0.1 → v1.0)
├── agents/
│   ├── structure-reviewer.md   — reviews plugin architecture and file placement
│   └── manifest-validator.md   — validates plugin.json and marketplace.json
└── references/
    ├── plugin-architecture.md  — full architecture guide (skills, agents, scripts, hooks, MCP)
    ├── growth-roadmap.md       — version milestones from MVP to publication
    ├── best-practices.md       — decision table, patterns, dependency direction
    └── security-checklist.md   — secrets, input validation, network safety
```

## Core Concept

```
Skill   = what should be done (workflow)
Agent   = who should do it (specialized expert)
Script  = how to check or calculate something exactly (deterministic)
Reference = long-term knowledge (checklists, rules, examples)
Hook    = automatic safety/quality rule
MCP     = external service/tool connection
```

## License

MIT

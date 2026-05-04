# Plugin Architecture Guide

## Core Concept

```
Skill = what should be done
Agent = who should do it
Script = how to check or calculate something exactly
Reference = long-term knowledge
Hook = automatic safety/quality rule
MCP = external service/tool connection
```

## Full Plugin Structure

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── main/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── routing.md
│   ├── audit/
│   │   ├── SKILL.md
│   │   ├── references/
│   │   │   └── audit-checklist.md
│   │   └── scripts/
│   │       └── collect.py
│   ├── technical/
│   │   └── SKILL.md
│   └── content/
│       └── SKILL.md
├── agents/
│   ├── technical.md
│   ├── content.md
│   ├── researcher.md
│   └── reporter.md
├── scripts/
│   ├── fetch.py
│   ├── parse.py
│   ├── validate.py
│   └── report.py
├── hooks/
│   ├── hooks.json
│   └── validate-output.py
├── references/
│   ├── quality-gates.md
│   ├── output-format.md
│   └── security.md
├── .mcp.json
├── settings.json
├── README.md
├── CHANGELOG.md
├── SECURITY.md
├── requirements.txt
└── tests/
    ├── test_fetch.py
    └── test_validate.py
```

## Flow

```
User command
  ↓
Main router skill
  ↓
Domain-specific skill
  ↓
Specialist agents (parallel or sequential)
  ↓
Deterministic scripts
  ↓
Reference knowledge
  ↓
Hooks and validation
  ↓
Optional MCP integrations
  ↓
Final user-facing output
```

## Skills

A skill is a workflow. It describes WHAT should happen.

### Main Router Skill

Every complete plugin should have a main skill that acts as a router. It receives the user command and delegates to the appropriate sub-skill.

### Workflow Skills

Each sub-skill represents one specific workflow. Keep SKILL.md focused on the workflow steps. Move long rules, checklists, and examples into `references/` files.

### Skill Frontmatter

Required fields:

```yaml
---
name: skill-name
description: >
  Detailed description of what this skill does and when to use it.
  Claude selects skills based heavily on this description.
argument-hint: "[target]"
user-invocable: true
---
```

### Key Rules

- Skills describe workflows, not contain all knowledge.
- Reference long rules with: `Read references/<file>.md`
- Use `$ARGUMENTS` for user input.
- Skills can instruct Claude to delegate to agents, but don't import agents directly.

## Agents

An agent is a specialized expert. It provides reasoning within a narrow domain.

### When to Use Agents

- Task is large and needs separate context
- Needs a specific expert role
- Needs different tool access
- Can be delegated from the main conversation
- Should not pollute main context

### Agent Frontmatter

```yaml
---
name: agent-name
description: >
  Specific expert role. What domain expertise. When Claude should delegate to this agent.
model: sonnet
maxTurns: 15
tools: Read, Grep, Glob, Bash
---
```

### Key Rules

- Give agents narrow, specific roles (not "general expert")
- Descriptions must explain WHEN to delegate
- Agents should specify output format
- An agent is NOT a slash command

### How Skills and Agents Connect

**Pattern A: Soft orchestration** (recommended)

In SKILL.md:
```
Delegate implementation review to the `technical` subagent.
```

Claude reads the skill and decides to call the agent.

**Pattern B: Skill runs inside agent**

In SKILL.md frontmatter:
```yaml
context: fork
agent: Explore
```

The entire skill runs in an isolated agent context.

## Scripts

Scripts are the deterministic layer. Use for exact behavior.

### When to Use Scripts

- Fetch a URL
- Parse HTML/JSON
- Validate structured data
- Call an API
- Calculate scores
- Generate reports
- Check file paths

### Script Requirements

- Accept CLI arguments
- Return JSON output
- Validate input (especially URLs and file paths)
- Use timeouts for network requests
- Return clear error messages
- Never hardcode secrets
- Be testable without Claude

### JSON Output Pattern

```json
{
  "ok": true,
  "target": "...",
  "data": {}
}
```

Error:

```json
{
  "ok": false,
  "error": "Description of what went wrong"
}
```

## Hooks

Hooks are automatic checks that run before or after Claude actions.

### Use Cases

- Validate JSON after file edits
- Run formatting after Write/Edit
- Block dangerous Bash commands
- Check generated schema
- Enforce output rules

### hooks.json Format

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"${CLAUDE_PLUGIN_ROOT}/hooks/validate-output.py\""
          }
        ]
      }
    ]
  }
}
```

### Hook Types

- `command`: execute shell command or script
- `http`: POST JSON to a URL
- `mcp_tool`: call an MCP tool
- `prompt`: evaluate with LLM
- `agent`: run an agent check

### Key Rules

- Scripts must be executable (`chmod +x`)
- All paths use `${CLAUDE_PLUGIN_ROOT}`
- Event names are case-sensitive: `PostToolUse`, not `postToolUse`

## MCP Integration

Use MCP when the plugin needs external tools or APIs.

### .mcp.json Format

```json
{
  "mcpServers": {
    "my-api": {
      "command": "node",
      "args": ["${CLAUDE_PLUGIN_ROOT}/servers/my-api-server.js"],
      "env": {
        "API_TOKEN": "${user_config.api_token}"
      }
    }
  }
}
```

### User Configuration

Define in plugin.json for secrets and user-specific settings:

```json
{
  "userConfig": {
    "api_token": {
      "type": "string",
      "title": "API token",
      "description": "Token for the external API",
      "sensitive": true,
      "required": true
    }
  }
}
```

## Reference Files

Store long knowledge in reference files:

- Checklists
- Rules and constraints
- Examples and templates
- Quality gates
- Output format specifications

In skills, reference them explicitly:

```
Read `references/checklist.md` before producing a final review.
```

## Environment Variables

- `${CLAUDE_PLUGIN_ROOT}` — plugin installation directory (changes on update)
- `${CLAUDE_PLUGIN_DATA}` — persistent data directory (survives updates)
- `${user_config.KEY}` — user configuration values

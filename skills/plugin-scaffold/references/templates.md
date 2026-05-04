# File Templates

## plugin.json

```json
{
  "name": "<plugin-name>",
  "version": "0.1.0",
  "description": "<one-line description of what the plugin does>",
  "author": {
    "name": "<author>"
  },
  "license": "MIT",
  "keywords": ["<keyword1>", "<keyword2>"]
}
```

## marketplace.json

```json
{
  "$schema": "https://anthropic.com/claude-code/marketplace.schema.json",
  "name": "<owner>-<plugin-name>",
  "owner": {
    "name": "<owner>"
  },
  "metadata": {
    "description": "<short description>"
  },
  "plugins": [
    {
      "name": "<plugin-name>",
      "source": "./",
      "description": "<detailed description>"
    }
  ]
}
```

## SKILL.md (router)

```md
---
name: main
description: Main router for <domain> workflows. Use for <list key workflows>.
argument-hint: "[command] [target]"
user-invocable: true
---

# <Plugin Name>

You are the router for this plugin.

## Commands

- `<command1> <target>` — <description>
- `<command2> <target>` — <description>

## Routing

When the user asks for <command1>:

1. Understand the target.
2. Load the <command1> workflow skill.
3. Delegate specialized work to the appropriate agent.
4. Use scripts for deterministic data collection.
5. Combine findings into a structured report.

## Output

Always return:

1. Executive summary
2. Critical findings
3. Important findings
4. Quick wins
5. Next actions
```

## SKILL.md (workflow)

```md
---
name: <workflow-name>
description: <Specific description of what this workflow does and when to use it.>
argument-hint: "[target]"
user-invocable: true
---

# <Workflow Name>

Run <workflow> for $ARGUMENTS.

## Steps

1. Collect data using scripts when possible.
2. Check against references/<checklist>.md.
3. Delegate expert review to the <role> agent.
4. Return prioritized findings.

## References

Read `references/<checklist>.md` for the full checklist.
```

## Agent

```md
---
name: <agent-name>
description: <Specific expert role. What domain expertise. When Claude should delegate to this agent.>
model: sonnet
maxTurns: 15
tools: Read, Grep, Glob, Bash
---

You are a senior <domain> specialist.

Your job:

1. Analyze the provided data or files.
2. Identify concrete issues with evidence.
3. Separate confirmed issues from assumptions.
4. Use scripts for deterministic checks when available.
5. Return concise, actionable findings.

For every finding include:

- severity (critical / warning / info)
- evidence
- explanation
- recommendation
```

## Script (Python)

```python
#!/usr/bin/env python3

import json
import sys


def main() -> None:
    if len(sys.argv) < 2:
        print(json.dumps({"ok": False, "error": "Usage: <script>.py <target>"}))
        sys.exit(1)

    target = sys.argv[1]

    try:
        result = {"ok": True, "target": target, "data": {}}
        print(json.dumps(result))
    except Exception as exc:
        print(json.dumps({"ok": False, "error": str(exc)}))
        sys.exit(1)


if __name__ == "__main__":
    main()
```

## hooks.json

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

## .mcp.json (with userConfig)

```json
{
  "mcpServers": {
    "<service-name>": {
      "command": "node",
      "args": ["${CLAUDE_PLUGIN_ROOT}/servers/<server>.js"],
      "env": {
        "API_TOKEN": "${user_config.api_token}"
      }
    }
  }
}
```

Corresponding userConfig in plugin.json:

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

---
name: create-skill
description: >
  Create a new skill for a Claude Code plugin or a standalone project.
  Generates SKILL.md with correct frontmatter and references directory.
  Works in two modes: plugin mode (skills/ inside a plugin) or project mode
  (.claude/skills/ for any project). Use when adding a new workflow or command.
argument-hint: "<skill-name> [in <path>]"
user-invocable: true
---

# Create Skill

Create a new skill named $ARGUMENTS.

## Before starting

Read `references/skill-guide.md` for the full guide on writing effective skills.

## Workflow

1. **Parse arguments.** Extract the skill name and optional target path. If no path given, use cwd.
2. **Detect mode.** Check if `.claude-plugin/plugin.json` exists at the target path.
   - **Plugin mode:** skill goes into `skills/<skill-name>/`
   - **Project mode:** skill goes into `.claude/skills/<skill-name>/`
3. **Ask the user** (if not obvious from context):
   - What does this skill do? (one sentence)
   - Should it auto-activate or only run on explicit invocation?
   - Does it need reference files? (checklists, patterns, examples)
   - Does it need an agent for delegation?
4. **Generate the skill directory:**

Plugin mode:
```
skills/<skill-name>/
├── SKILL.md
└── references/       # only if needed
    └── <topic>.md
```

Project mode:
```
.claude/skills/<skill-name>/
├── SKILL.md
└── references/       # only if needed
    └── <topic>.md
```

5. **Write SKILL.md** with:
   - Correct frontmatter (name, description, argument-hint, user-invocable)
   - Auto-activation triggers (if applicable)
   - Step-by-step workflow
   - References to any reference files
   - Delegation instructions to agents (if applicable)

6. **Update the router skill** (plugin mode only, if a router exists). Add the new command to the router's Commands list and Routing section.

7. **Show the result:** file tree, explanation, next steps.

## Rules

- Skill name must be kebab-case.
- Description must be specific enough for Claude to auto-select when relevant.
- Keep SKILL.md focused on workflow. Move long rules, checklists, and examples into `references/`.
- Never put all knowledge in SKILL.md — split into SKILL.md (workflow) + references (knowledge).
- Use `$ARGUMENTS` to reference user input.
- If the skill should delegate to an agent, use soft orchestration: `Delegate X to the <name> subagent.`

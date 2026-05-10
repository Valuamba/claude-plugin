# Skill Creation Guide

## What is a Skill

A skill is a workflow definition in a Claude Code plugin. It tells Claude **what should be done** — the steps, the order, what to check, and where to find knowledge.

A skill is NOT a knowledge base. Long rules, checklists, and examples belong in `references/` files.

## File Structure

```
skills/<skill-name>/
├── SKILL.md              # workflow definition
└── references/           # optional: long knowledge
    ├── patterns.md
    └── checklist.md
```

## SKILL.md Anatomy

### Frontmatter (required)

```yaml
---
name: my-skill
description: >
  What this skill does and when to use it. Be specific —
  Claude selects skills based on this description.
argument-hint: "<file path or target>"
user-invocable: true
---
```

| Field | Purpose |
|---|---|
| `name` | Kebab-case identifier. Becomes part of the slash command: `/plugin-name:skill-name` |
| `description` | How Claude decides whether to activate this skill. Write it like a search query match target. |
| `argument-hint` | Shows the user what to pass after the command. Use `<required>` and `[optional]`. |
| `user-invocable` | `true` = appears as a slash command. `false` = only called by other skills. |

### Auto-activation block (optional)

```markdown
## When to activate

Claude SHOULD invoke this skill automatically when the user:

- Asks to do X
- Mentions Y in context of Z
- Opens a file matching pattern P
```

This block makes the skill proactive — Claude will use it without the user typing the slash command.

### Workflow block (required)

```markdown
## Workflow

1. **Parse input.** Extract target from $ARGUMENTS.
2. **Read context.** Find relevant files or data.
3. **Apply rules.** Check against `references/checklist.md`.
4. **Delegate.** Pass specialized work to the `reviewer` subagent.
5. **Report.** Return structured findings.
```

### References block

```markdown
## References

Read `references/patterns.md` for the full pattern catalog.
Read `references/checklist.md` before producing the final report.
```

## Writing Good Descriptions

The description is the most important field. Claude uses it to decide when to activate the skill.

**Bad:**
```
description: Helps with code
```

**Good:**
```
description: >
  Review Python code that calls LLM APIs. Checks error handling,
  structured output usage, timeout patterns, and post-processing logic.
  Use when reviewing AI agent code quality.
```

Tips:
- Include the **domain** (Python, React, LLM, E2E tests)
- Include the **action** (review, generate, audit, scaffold)
- Include **trigger words** the user might say
- Keep under 3 lines

## Splitting Knowledge: SKILL.md vs References

| Goes in SKILL.md | Goes in references/ |
|---|---|
| Step-by-step workflow | Checklists with 10+ items |
| 3-5 core rules | Full pattern catalogs |
| When to activate | Example templates |
| Agent delegation instructions | Detailed scoring rubrics |
| Report structure outline | Signal tables and classification guides |

Rule of thumb: if a section is over 20 lines and is not a workflow step, extract it to a reference file.

## Connecting to Agents

Skills don't import agents directly. Instead, they instruct Claude to delegate:

```markdown
## Workflow

...
4. **Deep review.** Delegate implementation analysis to the `code-reviewer` subagent.
   Pass all found files and the checklist results.
```

Claude reads this and spawns the agent. The agent runs in its own context with its own tools.

## Connecting to Scripts

For deterministic operations, skills reference scripts:

```markdown
## Workflow

...
2. **Collect data.** Run `scripts/fetch.py <url>` to get page data.
3. **Parse.** Run `scripts/parse.py` on the raw HTML.
```

## Common Patterns

### Review skill

```
1. Find the target (file, module, URL)
2. Read and understand it
3. Check against reference checklist
4. Delegate deep analysis to agent
5. Compile structured report
```

### Generator skill

```
1. Parse user requirements from arguments
2. Read reference templates
3. Generate files following templates
4. Validate generated files
5. Show file tree and next steps
```

### Audit skill

```
1. Scan the target directory
2. Run deterministic checks via scripts
3. Delegate expert review to agent
4. Merge findings by severity
5. Return prioritized report
```

## Checklist Before Shipping

- [ ] `name` is kebab-case
- [ ] `description` is specific enough for auto-selection
- [ ] `argument-hint` shows expected input format
- [ ] Workflow has numbered steps
- [ ] Long knowledge is in `references/`, not inline
- [ ] Agent delegation uses soft orchestration pattern
- [ ] `$ARGUMENTS` is used for user input (not hardcoded)
- [ ] No secrets or API keys in any file

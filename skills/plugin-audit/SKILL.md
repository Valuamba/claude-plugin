---
name: plugin-audit
description: >
  Audit an existing Claude Code plugin for completeness, best practices, and common mistakes.
  Checks manifest, skills, agents, scripts, references, hooks, security, and structure.
  Use when reviewing a plugin before publishing or after making changes.
argument-hint: "[path]"
user-invocable: true
---

# Plugin Audit

Audit the plugin at $ARGUMENTS (or current directory if no path given).

## Workflow

1. Read the plugin directory structure.
2. Delegate structural review to the `structure-reviewer` agent.
3. Check each component against the checklist below.
4. Return a prioritized report.

## Checklist

### Manifest
- [ ] `.claude-plugin/plugin.json` exists
- [ ] `name` is kebab-case, no spaces
- [ ] `description` is specific and useful (not "helps with things")
- [ ] `version` follows semver
- [ ] `author` is set
- [ ] `keywords` are relevant

### Marketplace (if distributing)
- [ ] `.claude-plugin/marketplace.json` exists
- [ ] `name`, `owner`, `plugins` fields are present
- [ ] `source` path is correct

### Skills
- [ ] At least one skill exists in `skills/`
- [ ] Each skill has a `SKILL.md` with frontmatter (`name`, `description`)
- [ ] Descriptions are specific enough for Claude to select correctly
- [ ] Skills describe workflows, not embed all knowledge inline
- [ ] Long rules are in `references/` files, not in SKILL.md
- [ ] `$ARGUMENTS` placeholder is used for user input

### Agents
- [ ] Agents have narrow, specific roles
- [ ] Each agent has `name`, `description`, `model`, `tools` in frontmatter
- [ ] Agent descriptions explain WHEN to delegate to them
- [ ] Agents specify output format (severity, evidence, recommendation)

### Scripts
- [ ] Scripts accept CLI arguments
- [ ] Scripts return JSON output
- [ ] Scripts validate input (especially URLs and file paths)
- [ ] Scripts use timeouts for network requests
- [ ] Scripts have clear error messages
- [ ] No hardcoded secrets

### References
- [ ] Long checklists and rules are in reference files
- [ ] Skills reference them explicitly ("Read references/X.md")

### Hooks
- [ ] Hook scripts are executable (`chmod +x`)
- [ ] Paths use `${CLAUDE_PLUGIN_ROOT}`
- [ ] Event names are correct case (e.g., `PostToolUse`, not `postToolUse`)
- [ ] Matchers match intended tools

### Security
- [ ] No hardcoded API keys or tokens
- [ ] Sensitive config uses `"sensitive": true` in userConfig
- [ ] URL validation blocks `file://`, localhost, private IPs
- [ ] Scripts limit response sizes
- [ ] No `eval()` or shell injection vectors

### Structure
- [ ] Components are in the plugin root, NOT inside `.claude-plugin/`
- [ ] Only `plugin.json` and `marketplace.json` are inside `.claude-plugin/`

## Output

Return:

1. Overall health score (critical issues / warnings / passed)
2. Critical issues (must fix before publishing)
3. Warnings (should fix)
4. Passed checks
5. Recommended improvements

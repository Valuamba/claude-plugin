# Plugin Growth Roadmap

## Version 0.1 — MVP

Minimum viable plugin:

```
- plugin.json manifest
- one skill (SKILL.md)
- one agent
- one script (optional)
- README.md
```

Goal: prove the workflow works end-to-end.

## Version 0.2 — Structure

```
- main router skill
- 2-3 sub-skills for different workflows
- reference files for long rules
- structured output format definition
```

Goal: separate concerns between skills.

## Version 0.3 — Depth

```
- multiple specialized agents
- real scripts for data collection and validation
- unit tests for scripts
- input validation on all scripts
```

Goal: reliable, testable deterministic operations.

## Version 0.4 — Safety

```
- hooks for automatic validation
- report generation (structured output)
- persistent plugin data (${CLAUDE_PLUGIN_DATA})
- user configuration (userConfig)
```

Goal: automatic quality enforcement.

## Version 0.5 — Integration

```
- MCP server for external APIs
- API cost guards and budgets
- rate limit handling
- extension documentation
```

Goal: safe external service integration.

## Version 1.0 — Publication

```
- marketplace.json metadata
- CHANGELOG.md
- SECURITY.md
- installation guide
- usage examples
- version discipline (semver)
- LICENSE file
```

Goal: ready for public distribution.

## Assessment Criteria

| Component | Exists | Count |
|---|---|---|
| plugin.json | required | 1 |
| marketplace.json | for distribution | 1 |
| Skills | required | 1+ |
| Agents | recommended | 1+ |
| Scripts | for deterministic work | 0+ |
| References | for long knowledge | 0+ |
| Hooks | for auto-validation | 0+ |
| MCP config | for external APIs | 0-1 |
| Tests | for scripts | 0+ |
| README | recommended | 1 |
| CHANGELOG | for v1.0+ | 1 |
| SECURITY | for v1.0+ | 1 |

# Best Practices

## Keep skills small

Bad:
```
One SKILL.md with 2,000 lines of rules.
```

Good:
```
SKILL.md = workflow (under 100 lines)
references/*.md = long knowledge
scripts/*.py = exact operations
agents/*.md = specialized reasoning
```

## Give agents narrow roles

Bad:
```
agents/expert.md — "Helps with things"
```

Good:
```
agents/security-reviewer.md — security and vulnerability analysis
agents/performance-analyst.md — performance metrics and optimization
agents/report-writer.md — structured report generation
```

## Write strong descriptions

Claude selects skills and agents based heavily on their descriptions.

Bad:
```yaml
description: Helps with things.
```

Good:
```yaml
description: >
  Use this agent for technical SEO analysis, including crawlability,
  indexability, redirects, canonicals, robots.txt, sitemap issues,
  and Core Web Vitals.
```

## Scripts return structured JSON

Good:
```json
{
  "ok": true,
  "issues": [
    {
      "severity": "critical",
      "title": "Missing required field",
      "evidence": "Field `name` was not found",
      "recommendation": "Add the required `name` field"
    }
  ]
}
```

Bad:
```
Error: something went wrong
Found 3 issues
```

## Never hardcode secrets

Bad:
```
API_KEY=abc123
```

Good:
```
userConfig with sensitive: true
environment variables
system keychain
```

## Validate external input

Minimum protections for URLs:

- Allow only http/https
- Block file://
- Block localhost and 127.0.0.1
- Block private IP ranges (10.x, 172.16-31.x, 192.168.x)
- Block cloud metadata endpoints (169.254.169.254)
- Add request timeouts (10-30s)
- Limit response size
- Sanitize file paths (no ../ traversal)

## Dependency direction

```
Skills orchestrate workflows
  → Agents provide expert reasoning
    → Scripts perform exact computation
      → References provide knowledge

Never invert this direction.
```

- Skills can reference agents and scripts.
- Agents can use scripts.
- Scripts must not invoke skills or agents.
- References are passive knowledge, read by skills and agents.

## Decision table

| Need | Use |
|---|---|
| Repeatable workflow | Skill |
| Main command router | Main skill |
| Specialized expert | Agent |
| Large isolated reasoning task | Agent |
| Exact calculation or validation | Script |
| External API | MCP or script |
| Long rules/checklists | Reference file |
| Automatic validation | Hook |
| User secrets | Sensitive user config |
| Persistent data | Plugin data directory |
| Distribution | Plugin package + marketplace |
| Quick local experiment | `--plugin-dir` |

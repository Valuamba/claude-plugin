# Routing Reference

## Command Resolution

| User input | Skill | Agent | Notes |
|---|---|---|---|
| `scaffold <name>` | plugin-scaffold | — | Generates full directory |
| `scaffold <name> --minimal` | plugin-scaffold | — | MVP: manifest + 1 skill + 1 agent |
| `create skill <name>` | create-skill | — | Adds skill to cwd plugin |
| `create skill <name> in <path>` | create-skill | — | Adds skill to plugin at path |
| `audit` | plugin-audit | structure-reviewer | Audits cwd plugin |
| `audit <path>` | plugin-audit | structure-reviewer | Audits plugin at path |
| `grow` | plugin-grow | — | Growth guidance for cwd plugin |
| `grow <path>` | plugin-grow | — | Growth guidance for plugin at path |
| (empty) | — | — | Interactive questionnaire |

## Namespace Rules

Plugin name in `plugin.json` determines the command namespace:

```
plugin name: "my-domain"
skill name: "audit"
user command: /my-domain:audit
```

The main skill (router) uses the plugin name directly:

```
/plugin-create scaffold my-seo-plugin
/plugin-create audit ./my-plugin
/plugin-create grow
```

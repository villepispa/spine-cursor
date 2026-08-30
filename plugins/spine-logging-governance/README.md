# spine-logging-governance

Cursor plugin (Spine Framework wave 1): **AL** (agent log) and **AAL** (auto-approval log)
discipline — newest-first tables, sentinel markers, scope resolution.

## Install (local)

```text
New-Item -ItemType SymbolicLink -Force `
  -Path "$env:USERPROFILE\.cursor\plugins\local\spine-logging-governance" `
  -Target "<path-to-clone>\plugins\spine-logging-governance"
```

Reload the Cursor window.

## Contents

| Path | Role |
|------|------|
| `rules/logging.mdc` | When and where to log |
| `skills/logging/` | Schemas and insertion pattern |
| `templates/log-table-skeleton.md` | Empty AL/AAL headers + sentinels |

**Adaptation:** Create your own `logs/` paths in the consumer workspace. Do not
ship historical log rows. Sentinel example: `<!-- AL+ -->`.

## Soft dependencies

Works alone; pairs well after `spine-agent-file-operations`.

## License

MIT — see repository root `LICENSE`.

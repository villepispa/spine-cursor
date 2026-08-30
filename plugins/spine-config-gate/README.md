# spine-config-gate

Cursor plugin (Spine Framework wave 1): **Sphinx gate** checklist before committing
substantial changes to rules, skills, agents, docs, templates, commands, or hooks.

## Install (local)

```text
New-Item -ItemType SymbolicLink -Force `
  -Path "$env:USERPROFILE\.cursor\plugins\local\spine-config-gate" `
  -Target "<path-to-clone>\plugins\spine-config-gate"
```

Reload the Cursor window.

## Contents

| Path | Role |
|------|------|
| `rules/sphinx-gate.mdc` | Gate checklist (conflict, coverage, format, scope, …) |
| `agents/setup-buddy-lite.md` | Short agent: gate + PCB reminder |

## Soft dependencies

Best after `spine-agent-file-operations` and `spine-logging-governance`.

## License

MIT — see repository root `LICENSE`.

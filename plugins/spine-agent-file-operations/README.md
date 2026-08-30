# spine-agent-file-operations

Cursor plugin (Spine Framework wave 1): keep agents on **Read / Grep / Glob / Write / StrReplace**
for file work instead of Shell `Get-Content` / `Set-Content` / `cat` / `grep`.

## Install (local)

```text
# Windows (PowerShell) — symlink into Cursor local plugins
New-Item -ItemType SymbolicLink -Force `
  -Path "$env:USERPROFILE\.cursor\plugins\local\spine-agent-file-operations" `
  -Target "<path-to-clone>\plugins\spine-agent-file-operations"
```

Reload the Cursor window. Confirm under Settings → Rules / Skills.

## Contents

| Path | Role |
|------|------|
| `rules/agent-file-operations.mdc` | Always-apply decision tree |
| `skills/agent-file-operations/` | PS7 / audit patterns |
| `skills/agent-file-operations-violation-triage/` | Triage when an agent broke the rule |

Long-form guide (optional reading): not bundled in v0.1 — see the Spine docs lane
on GitHub when published separately.

## Soft dependencies

Install order: **this plugin first**, then `spine-logging-governance`, then
`spine-config-gate`.

## License

MIT — see repository root `LICENSE`.

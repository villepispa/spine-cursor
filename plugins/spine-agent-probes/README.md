# spine-agent-probes

Cursor plugin (Spine): **`-AgentSummary`** one-line outcomes and **`-Json`**
envelope convention for allowlist-safe PowerShell helpers.

## Install (local)

```text
New-Item -ItemType SymbolicLink -Force `
  -Path "$env:USERPROFILE\.cursor\plugins\local\spine-agent-probes" `
  -Target "<path-to-clone>\plugins\spine-agent-probes"
```

Reload the Cursor window. Confirm under Settings → Skills.

## Contents

| Path | Role |
|------|------|
| `docs/agent-summary-probe-contract.md` | Full public contract (forum / GitHub SSOT) |
| `skills/agent-summary-probe-contract/` | Agent skill — when and how to apply the switches |

## Soft dependencies

Best after `spine-agent-file-operations` (Shell tool choice). Pairs with
`spine-config-gate` for `**Safety tier: N**` on scripts.

## Forum / deep link

Canonical contract path in this repo:

`plugins/spine-agent-probes/docs/agent-summary-probe-contract.md`

## License

MIT — see repository root `LICENSE`.

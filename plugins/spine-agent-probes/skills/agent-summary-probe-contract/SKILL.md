---
description: >-
  PowerShell probe dialect for Cursor agents: -AgentSummary one-line outcomes,
  -Json envelopes, exit twin (OK vs FAIL vs optional MISS), Safety tier line,
  nested-helper switch threading. Use when writing or reviewing agent-runnable
  Invoke-*.ps1 gates/probes, or when Shell allowlist compounds hide exit codes.
framework:
  id: agent-summary-probe-contract-skill
  kind: skill
  scope: operations
  topic: governance
  depends_on:
    - agent-summary-probe-contract
    - framework-overview
  triggers:
    - AgentSummary
    - -Json probe
    - probe contract
    - PowerShell gate summary
  status: active
---

# AgentSummary probe contract

**Full reference:** [agent-summary-probe-contract.md](../../docs/agent-summary-probe-contract.md)

**Sibling plugins:** file-ops (when Shell is the right tool);
config-gate (`**Safety tier: N**` + runtime hold for untiered scripts).

## When to use

- New or changed `scripts/Invoke-*.ps1` meant for agent Shell (`pwsh -File`)
- Gates/probes where compound `if ($LASTEXITCODE…)` would prompt the allowlist
- Prefer one summary line / small JSON over dumping verbose CLI into chat

## Quick rules

1. Declare `[switch]$Json` and `[switch]$AgentSummary` (or document
   `# AgentSummary: skip — <rationale>`).
2. On `-AgentSummary`: emit **exactly one** success-stream line
   (`PREFIX-OK` / `PREFIX-FAIL exit=N` / optional `PREFIX-MISS*`).
3. **Exit twin:** process failure → non-zero + FAIL/MISS; successful domain
   reports (empty tree, `exists:false`) stay exit 0 + OK unless the script is a
   **gate**.
4. Both switches: print envelope JSON **then** the summary line.
5. Nested helpers take **only** the switches they use; pass at every call site.
6. Open `.DESCRIPTION` with `**Safety tier: N**` (blank help line after).
7. Exit taxonomy: `0` clean, `1` failed, `2` missing dependency.

## Envelope shape

```json
{
  "ok": true,
  "exitCode": 0,
  "safetyTier": 1,
  "summary": "VALIDATOR-OK",
  "data": {}
}
```

## Do not

- Treat inventory "not found" as `*-FAIL` when the probe successfully reported it
- Emit FAIL only on stderr (agents need the summary line before `exit`)
- Declare unused `-Json` on a summary-only nested helper

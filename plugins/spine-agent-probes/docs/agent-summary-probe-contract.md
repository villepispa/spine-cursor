---
framework:
  id: agent-summary-probe-contract
  kind: reference
  scope: adaptation
  topic: governance
  depends_on:
    - framework-overview
  triggers:
    - AgentSummary
    - probe contract
    - PowerShell agent automation
  status: active
---

# AgentSummary and the PowerShell probe contract

**Audience:** Agents and humans building allowlist-safe PowerShell helpers for
Cursor (or similar agent IDEs).

**Plugin:** Ship this file with skill
`skills/agent-summary-probe-contract/` in the `spine-agent-probes` Cursor plugin.

## 1. Problem

Agents invoke helpers with a single Shell call:

```powershell
pwsh -NoProfile -File scripts/Invoke-SomeGate.ps1
```

Compound exit handling (`…; if ($LASTEXITCODE -ne 0) { … }`) often triggers
extra allowlist prompts and hides outcomes from humans watching the chat.

Verbose terminal dumps also burn agent context. When the IDE lacks terminal
output compression for agents, prefer one summary line and/or a small JSON
envelope over pasting interactive CLI transcripts into chat.

## 2. `-AgentSummary`

Opt-in switch on agent-runnable gates and probes. When set, write **one**
success-stream line after primary work:

```text
VALIDATOR-OK
LINT-OK findings=0
LINT-FAIL exit=1 findings=12
LINT-MISS
MDLINT-MISS
YAMLLINT-MISS
JSONSCHEMA-MISS
VALE-MISS
VALE-MISS-STYLES
```

| Part | Convention |
|------|------------|
| Prefix | Role-derived `SCREAMING-KEBAB` |
| Success | `<PREFIX>-OK` + optional `key=value` |
| Failure | `<PREFIX>-FAIL exit=<code>` |
| Missing (optional) | `<PREFIX>-MISS` or `<PREFIX>-MISS-<DETAIL>` on exit 2 when agents need distinct bootstrap remediations; default remains `*-FAIL exit=2` |
| Stream | `Write-Output` |
| Exit twin | Mirrors **exit code**, not "happy path only." Process failure → non-zero + `*-FAIL` (or optional `*-MISS*` on exit 2). Domain probe results (empty tree, `exists:false`) stay exit 0 + `*-OK` unless the script is a **gate**. Catch throws so FAIL/MISS is emitted before `exit`. |

When both `-Json` and `-AgentSummary` are set, emit envelope JSON then the
summary line (mixed stdout; parsers should tolerate JSON followed by one
non-JSON trailing line). `-AgentSummary` alone remains summary-only.

### 2.1 Exit twin — when to emit OK vs FAIL

The row above summarises the rule; the situation table below is the decision
surface an agent can traverse row-by-row.

| Situation | Exit | AgentSummary |
|-----------|------|--------------|
| Probe ran; payload *is* the answer (empty tree, `exists: false`, `count=0`) | `0` | `*-OK` (+ metrics) |
| Script could not do its job (bad roots, I/O, missing dependency) | `1` / `2` | `*-FAIL exit=N` |
| Missing tool / pack with **distinct** agent remediations (install vs sync) | `2` | `*-MISS` or `*-MISS-<DETAIL>` (optional; else `*-FAIL exit=2`) |
| Gate found policy violations (lint, validator) | `1` | `*-FAIL` with finding counts |

**Probes vs gates.** Inventory helpers (hash audit, path metadata, line
stats) stay `*-OK` when they successfully *report* a missing folder or file.
Treating those domain results as `*-FAIL` trains agents to think the probe
broke when it worked — the strongest exit-twin anti-pattern. Reserve `*-FAIL`
for **process** failure; wrap throws so a summary line is always emitted
before `exit` (never stderr-only).

Gates (validators, linters, safety checks) correctly use `*-FAIL` when
findings violate policy; metrics like `findings=12` stay on the FAIL line
(or in JSON envelope `data` when `-Json` is set).

**`*-MISS*` (optional).** Use when exit 2 has two or more remediations agents
must distinguish without parsing JSON (for example CLI missing vs style pack
missing). Use plain `*-FAIL exit=2` when one remediation covers all
missing-dependency cases.

## 3. Reserved switches (probe contract)

| Switch | Role |
|--------|------|
| `-Json` | Envelope JSON on stdout (alone or with `-AgentSummary`) |
| `-AgentSummary` | One summary line (alone or trailing after JSON) |
| `-RepoRoot` / `-Path` | Targets |
| `-WhatIf` / `-Confirm` | Mutators |

Envelope shape:

```json
{
  "ok": true,
  "exitCode": 0,
  "safetyTier": 1,
  "summary": "VALIDATOR-OK",
  "data": {}
}
```

Exit taxonomy: `0` clean, `1` failed, `2` missing dependency.

**Nested exit helpers:** when a nested function writes the envelope or summary,
declare **only the switches that helper uses** and pass them at every call site.
Envelope writers typically take both (`-Json:$Json -AgentSummary:$AgentSummary`).
Helpers that only emit the summary after the caller already wrote JSON should
take `-AgentSummary` alone — do not declare unused `-Json` on the helper. Explicit
parameters keep the contract readable and unused-parameter lint quiet.

## 4. Safety tier

Comment-based help opens `.DESCRIPTION` with `**Safety tier: N**` (1 read-only,
2 controlled write/network, 3 never in catalog). Pair with a runtime gate before
`pwsh -File` so untiered scripts are held as external/unsafe until sealed.

## 5. Lint enforcement

Catalog lint should Error when `scripts/Invoke-*.ps1` lacks
`[switch]$AgentSummary` **and** lacks `# AgentSummary: skip — <rationale>`.
Exclude drafts, archive, hooks, and library shards.

## 6. Cousins (not clones)

- One JSON line on stdout / logs on stderr
- Global `--agent` CLI mode
- TAP `ok` / `not ok`
- GitHub Actions `::error` workflow commands
- Tool wrappers that emit one summary line
- Cursor official
  [cli-for-agent](https://github.com/cursor/plugins/tree/main/cli-for-agent)
  skill **Success output** (machine-useful IDs/URLs/durations on success) —
  same family as one-line gate summaries; not a PowerShell switch

Distinctive here: PowerShell **`[switch]$AgentSummary`** for Cursor allowlist UX
and human-skimmable gate lines.

## Change history

- 2026-07-27 — Published under `spine-agent-probes` plugin (`SPC-003`).
- 2026-07-27 — Cousins += official Cursor `cli-for-agent` Success output.
- 2026-07-27 — Context-budget / missing IDE terminal-compression framing in § 1;
  nested helpers take only switches they use (§ 3).
- 2026-07-23 — Example `*-MISS*` tokens; nested exit-helper threading; dual
  stdout ordering.
- 2026-07-22 — § 2.1 exit-twin situation table + probes-vs-gates.
- 2026-07-21 — Initial public write-up; **Exit twin** row.

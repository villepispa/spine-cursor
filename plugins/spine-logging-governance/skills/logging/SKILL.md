---
description: >-
  Logging skill — where and how to write AL, AAL, and chat log entries.
  Covers scope resolution (global/project/subproject), column schemas for all log types,
  and auto-approval chain.   Use when the logging.mdc rule applies and you need
  the detailed schemas and paths.
framework:
  id: logging-skill
  kind: skill
  scope: operations
  topic: change-management
  depends_on: [logging, logs-principles, framework-overview]
  triggers: [logging SKILL, AL AAL schemas]
  status: active
---

# Logging skill

**Principles:** [logs-principles.md](../../docs/principles/logs-principles.md)
— why AL, AAL, chat log, VM logs, and CL+ exist as traceability artefacts.

**Where/how** to **AL** and **AAL**. **MUST:** append readable rows so user/auditor sees what, who, why.

## Scope and paths

| Log | Path |
|-----|------|
| **Global agent log** | `.cursor/logs/agent-log-global.md` |
| **Project agent log** | `<repo-root>/.cursor/agent-log.md` |
| **Subproject agent log** | `<subproject-path>/.cursor/agent-log.md` |
| **Global auto-approval log** | `.cursor/logs/auto-approval-log-global.md` |
| **Project auto-approval log** | `<repo-root>/.cursor/auto-approval-log.md` |
| **Chat log** | `.cursor/logs/cursor-chat-log.md` |

**Resolution:** single file / no resolved project → **global only**. Folder + resolved project/subproject → project or subproject **and** global.

## Column schemas

Ordering, timestamps, encoding, insertion: [log-writing-conventions.md](../../docs/governance/log-writing-conventions.md).

### Timestamps

**PREFER:** format the IDE `<timestamp>` block to `YYYY-MM-DD HH:mm:ss` for same-turn rows — **zero Shell**, one value **reused** across CL+, AL, AAL, chat log, and AAI in that turn (`:00` when seconds are omitted).

**Shell** (`Get-Date -UFormat …` / `date …`) only for UTC audit marks (AskQuestion premark/postmark), retroactive catch-up, or a single refresh at the end of a long turn. **NEVER:** a standalone Shell invocation whose only purpose is fetching time before log edits.

Full rules: [log-writing-conventions.md § 2](../../docs/governance/log-writing-conventions.md).

### Project / subproject `agent-log.md`

| Column | Content |
|--------|---------|
| **When** | Per [log-writing-conventions.md § 2](../../docs/governance/log-writing-conventions.md) |
| **Agent** | e.g. VM, PM, cpp-reviewer, changelog |
| **Action** | Short verb phrase |
| **Scope** | Path or context |
| **What** | 1–2 lines |
| **Why** | Trigger / reason |
| **LLM** | `[Tier N]` or `[Tier N: Model]` per [llm-selection.mdc](../../rules/llm-selection.mdc) |

New file: title `# Agent log — <name>`, then standard header and separator rows.

### Global `logs/agent-log-global.md`

`| When | Scope (project:/subproject:/file:) | Agent | Action | Summary | Reference | LLM |`

**Scope format**: `file: <path>` (single file) · `<path>, <path>` (multiple) · `.cursor/<area>` (config) · `workspace: <name>` (workspace-wide).

**Reference format**: `CHANGELOG YYYY-MM-DD HH:mm:ss` · `[CHANGELOG](…) YYYY-MM-DD` · prose (`User confirmed …`) · `—` (global-only).

**PREFER:** anchors (`#entry-…`) in Reference when practical.

### Chat log `logs/cursor-chat-log.md`

`| When | Workspace | Topic | Summary |`

One row per config-related conversation; insert newest first.

**Model slug convention:** when the conversation involved substantive AI work,
end **Summary** with `| Models: <slug>` (comma-separated pinned slugs from
[llm-selection-slug-reference.md](../../docs/llm-selection/llm-selection-slug-reference.md)).
Omit for trivial or read-only turns.

### AI assistance log (AAI) — project disclosure

Human-facing disclosure for cross-vendor audit and thesis/ethics annexes.
Operational AL rows stay mechanical; AAI rows capture **what** was assisted,
**which model**, and **why** per phase.

| Log | Path (first match wins) |
|-----|-------------------------|
| Config-style workspace | `<repo-root>/logs/ai-assistance-log.md` |
| Product / research repo | `<repo-root>/docs/ai-assistance-log.md` |
| Minimal repo | README § **AI assistance** (link to full log when created) |

**Bootstrap:** copy [`templates/ai-assistance-log-template.md`](../../templates/ai-assistance-log-template.md)
when starting a new long-lived workspace. Graded COMP.CS.115 source uses
[`ai-assisted-comment-template.md`](../../templates/ai-assisted-comment-template.md)
instead (code headers, not AAI).

#### AAI column schema (canonical six)

`| When | Phase / scope | Deliverable | Tool / model | Purpose | Verified by |`

Optional domain columns (Role, Paths, Human verification) — see template § Optional columns.

**When to append:** end of each substantive phase (plan todo, epic slice, publication
draft, major feature) — not every chat turn. Same turn as the matching AL row when both apply.

**Tool / model cell:** `Cursor Agent [Tier N: model-slug]` — slug from slug reference;
tier-only only when slug was not exposed (note in Purpose; backfill from transcripts when auditing).

**Sentinel:** `<!-- AAI+ -->` — newest first; same StrReplace insertion pattern as AL.

## Process-friction

Guide: [agent-process-friction-reports.md](../../docs/agent/agent-process-friction-reports.md).
Rule trigger: [logging.mdc](../../rules/logging.mdc) § 5.

**When:** phase boundary with non-empty process friction (skills / rules /
allowlist / hand-offs / docs). **SKIP** if none. **NEVER** treat as licence to
edit policy without user request.

**AL Action value:** `process-friction`

| Column | Content |
|--------|---------|
| **What / Summary** | Compressed *What* + *Improve* (≤ ~160 chars) |
| **Why** (project log) / **Reference** (global) | Trigger (`phase close`, `subagent aggregate`, backlog ID) · guide path optional |
| **Scope** | Paths or surfaces that hurt; note `subagent:<type>` when children contributed |

**Chat:** same-turn `### Process friction` block when non-empty. Config chats:
mention `friction: …` in chat-log Summary.

**Ask-then-continue:** when a mid-phase guidance ask was used, include
outcome token in Summary / Guidance line:
`ask-answered` | `ask-continue-default` | `ask-unanswered-default`.
Policy: [guide § Ask-then-continue](../../docs/agent/agent-process-friction-reports.md#ask-then-continue).
**PREFER** markdown ask; **NEVER** treat AskQuestion skip-class as continue.

**Subagents:** parent synthesizes child returns into one AL row; children report
in their Output contract only unless they have a project log in-scope.
Guidance asks escalate to parent (children do not own the wait).

**Not AAL.** Backlog paste template:
[`templates/agent-process-friction-backlog-proposal.md`](../../templates/agent-process-friction-backlog-proposal.md).

## Auto-approval (AAL)

When action is auto-approved: **in addition to AL**, append AAL.

**Decision rule (one sentence):** *AAL = the audit row for actions where no human said yes at the moment of execution.* Everything else logs to AL only.

**Chain:** subproject `.cursor/auto-approval-log.md` (full) → project (xref) → global `logs/auto-approval-log-global.md` (xref).

### DOES count (write AAL)

| Trigger | Where it shows up | AAL `Action` value |
|---------|-------------------|--------------------|
| Tier 1 `permissions.json` auto-insert | `docs/governance/permissions-approvals.md` `auto-inserted` row, no user prompt | `auto-inserted` |
| Tier 3 `permissions.json` auto-removal under approved-remove policy | `docs/governance/permissions-approvals.md` `auto-removed` row | `auto-removed` |
| `autoAcceptTaskSuggestionsFrom` config consumes a suggestion without prompt | Cursor IDE setting (currently dormant) | `config-auto-accepted` |
| Agent autonomous bypass of normal gating | Plan body cites the case + AL row | `agent-decision` |

### Does NOT count (AL only)

| Action | Why it is not auto-approval |
|--------|-----------------------------|
| Tier 2 `permissions.json` `approved-add` / `approved-remove` after `AskQuestion` or chat consent | Explicit user approval at the moment of the change |
| Allowlisted Shell command run (e.g. `git status`, `pwsh …`) | Pre-approved by config; the **policy** is the approval, not each invocation |
| User-initiated slash command (`/backlog`, `/markdown-to-pdf`, …) | Explicit user request initiates the work |
| User saying "implement", "do it", "go ahead", "implement-all" in chat | Explicit user request, even when batch-shaped |
| File-ops triage initiated by `/agent-file-operations-violation-triage` | Explicit user action (paste + invoke) |

### AAL column schema

`| When | Scope | Agent | Action | What | Reason (Config | User requested | Agent decision) | Reference |`

Append-only (no deletion of existing rows). Corrections / reclassifications go in-place in the **Reason** cell with a `[reclassified YYYY-MM-DD: …]` annotation; the original row is **not** deleted (per [logs-principles § 3](../../docs/principles/logs-principles.md)).

### Example AAL row (Tier 1 auto-insert)

```text
| 2026-04-30 06:18:52 | file: permissions.json | Cursor AI | auto-inserted | Tier 1 allowlist auto-insert: `wsl --status` (read-only WSL default version / inbox status) | Agent decision (Tier 1 prefix not previously covered; user requested wsl status check) | docs/governance/permissions-approvals.md row 2026-04-30 06:18:52 |
```

## Insertion method — sentinel-based StrReplace

Every log file has a unique HTML comment sentinel between the separator row and the first data row. Use it as the StrReplace anchor — zero terminal prompts.

| Log / Table | Sentinel | Notes |
|-------------|----------|-------|
| CHANGELOG.md | `<!-- CL+ -->` | Newest first |
| _archive/CHANGELOG-archive.md | `<!-- CL-ARCHIVE+ -->` | Between static sections and first rolled batch; Scribe prepends new `## Rolled…` blocks here. Rows within each batch are newest-first. |
| agent-log-global.md | `<!-- AL+ -->` | Newest first |
| cursor-chat-log.md | `<!-- CHATLOG+ -->` | Newest first |
| version-management-global-log.md | `<!-- VMLOG+ -->` | Newest first |
| auto-approval-log*.md | `<!-- AAL+ -->` | Newest first |
| ai-assistance-log.md (`logs/` or `docs/`) | `<!-- AAI+ -->` | Newest first; project disclosure |
| Project/subproject agent-log.md | `<!-- AL+ -->` | Newest first |
| forum/cursor/issues/README.md status table | `<!-- ISSUES+ -->` | Newest first |
| plans/README.md active table | `<!-- PLANS-ACTIVE+ -->` | Newest first |
| plans/README.md done table | `<!-- PLANS-DONE+ -->` | Newest first |
| docs/backlog.md per-epic tables | `<!-- BL: <epic-id> -->` | e.g. `<!-- BL: frm-guides -->` — see backlog for all IDs |
| `_archive/archive-log.md` | `<!-- ARCHIVE-LOG+ -->` | Newest first; Loom / archive workflow |
| logs/file-ops-violation-log.md | `<!-- FOPS-VIOLATION+ -->` | Newest first; triage entries only — counts use direct StrReplace on each row label |
| logs/agent-compatibility-log.md | `<!-- ACLOG+ -->` | Newest first; one row per `/check-agent-compatibility` scan — skill MUST log same turn as score |

**Steps:**

1. `Grep` the sentinel in the target file to confirm location and read the current first data row.
2. `StrReplace`: old = sentinel line + first data row; new = sentinel + NEW row + first data row.

```text
old_string = "<!-- AL+ -->\n| 2026-03-30 12:03:39 | ..."
new_string = "<!-- AL+ -->\n| 2026-03-30 12:16:55 | NEW ROW ... |\n| 2026-03-30 12:03:39 | ..."
```

NEVER: `Add-Content` (appends to EOF; all logs are newest-first). Shell `Get-Content`/`Set-Content` reimplementing StrReplace.

**New file creation:** title `# Log name`, then header row, separator row, sentinel, and first data row. Always include the sentinel.

### Cross-workspace writes — `Insert-LogRow.ps1`

**Problem:** When the current workspace is not `~/.cursor/` (e.g. a course project), Cursor's External-File Protection blocks `StrReplace`/`Write` for the global log — prompting on every write.

**Solution:** Use the helper script via terminal. `pwsh` is on the command allowlist → zero prompts.

**Decision rule:**

| Current workspace | Method for global log |
|---|---|
| `~/.cursor/` (config workspace) | `StrReplace` with sentinel (preferred — zero prompts, standard method) |
| Any other workspace | `pwsh -File Insert-LogRow.ps1` (bypasses External-File Protection via terminal) |

**Command template:**

```powershell
pwsh -File "<config-workspace>\hooks\Insert-LogRow.ps1" -LogFile "logs/agent-log-global.md" -Sentinel "<!-- AL+ -->" -Row "| TIMESTAMP | SCOPE | AGENT | ACTION | SUMMARY | REFERENCE | [Tier N] |"
```

Replace `logs/agent-log-global.md` and `<!-- AL+ -->` with the appropriate log file and sentinel for other logs (e.g. `logs/auto-approval-log-global.md` / `<!-- AAL+ -->`).

**This is a scoped exception** to the "NEVER reimplements StrReplace" rule. Rationale: StrReplace is blocked by External-File Protection for cross-workspace paths; the helper script is tested, path-validated, and encoding-safe. The exception applies **only** to global log writes from non-config workspaces.

**Script guarantees:**

- Path-traversal guard: `-LogFile` must resolve inside `~/.cursor/`.
- Refuses to create new files (sentinel must already exist).
- Preserves UTF-8 no-BOM encoding and line endings.
- Exits non-zero on any error (sentinel not found, path violation, missing file).

**Project-local logs** (`<repo>/.cursor/agent-log.md`) are inside the workspace — always use `StrReplace` for those.

## Flow

1. After effectful action, resolve scope.
2. Back up if doing maintenance (see [agents/changelog.md § Backup protocol](../../agents/changelog.md)).
3. Append project/subproject row if applicable.
4. Append global row with xref to detailed row or `—`.
5. **MUST:** create file with standard header + sentinel if missing.

## Optional config overrides

- **logFileEncoding** — overrides default UTF-8
- **agentLogGlobalFile** → replaces default global path
- **agentLogFile** / **agentLogSubprojectFile** → project/subproject paths

## Reference

- Rule: → [logging.mdc](../../rules/logging.mdc)
- Log steward: → [agents/changelog.md](../../agents/changelog.md) (Scribe — backup, rolling, audit, bootstrap)
- Conventions: → [log-writing-conventions.md](../../docs/governance/log-writing-conventions.md)
- Project resolution: → [version-management.md](../../docs/version-management/version-management.md)

Change history: [CHANGELOG.md](../../CHANGELOG.md)

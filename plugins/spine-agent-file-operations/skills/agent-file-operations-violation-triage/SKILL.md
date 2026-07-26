---
name: agent-file-operations-violation-triage
description: >-
  Triages user reports that an agent ignored `docs/agent/agent-file-operations-guide.md`
  (and the short rule `rules/agent-file-operations.mdc`): maps evidence to guide
  sections, classifies violation vs doc gap vs false positive, proposes or applies
  doc/rule fixes when warranted, and **always** returns a structured analysis and
  next actions to the user. Use when the user says an agent used Shell for edits,
  used `Get-Content`/`grep`/`cat` instead of Read/Grep, bare `$x = …` Shell,
  `powershell.exe`, wrong log sentinels, `cmd /c`, or otherwise broke file-ops discipline.
framework:
  id: agent-file-operations-violation-triage-skill
  kind: skill
  scope: operations
  topic: governance
  depends_on:
    - agent-file-operations-guide
    - agent-file-operations
    - agent-behavior-and-context
  triggers:
    - agent violated file operations
    - report shell misuse
    - Get-Content instead of Read
  status: active
---

# Agent file-operations violation triage

**Canonical specs:** [agent-file-operations-guide.md](../../docs/agent/agent-file-operations-guide.md) (long form) and [agent-file-operations.mdc](../../rules/agent-file-operations.mdc) (alwaysApply decision tree). **Shell audits** (paired trees, JSON inventory): [agent-file-operations/SKILL.md](../agent-file-operations/SKILL.md).

## When this applies

User (or reviewer) reports that an **agent session** did not follow the guide: wrong tools, forbidden Shell patterns, bad anchors, encoding or sentinel mistakes, etc.

## Intake (gather before judging)

Ask or extract from the user message:

| Field | Why |
|--------|-----|
| **What happened** | Observed agent behavior (commands, tools, sequence). |
| **Evidence** | Transcript path, chat excerpt, screenshot, or copy of Shell lines. Include the confirmation UI **Reason** line when the report is about unexpected **Run** prompts (`Not in allowlist: …` → [cursor-command-allowlist.md § 1](../../docs/agent/cursor-command-allowlist.md)). |
| **Workspace** | `.cursor` config repo vs course repo vs other (affects `.gitignore` §0). |
| **Approx. date / Cursor version** | If known — helps correlate with rule versions. |

If evidence is missing, **still** deliver the analysis template with a **Needs input** block listing the minimum fields; do not invent facts.

## Analysis workflow

1. **Read** the current [agent-file-operations-guide.md](../../docs/agent/agent-file-operations-guide.md) **§ Quick reference — tool substitution**, **§ 0a** (*large `Write` / empty-tool / transport failures*), and the **NEVER** list in [agent-file-operations.mdc](../../rules/agent-file-operations.mdc) for the reported action.
2. **Map** the report to one or more **guide sections** (use section numbers / titles from the guide TOC, e.g. §0 gitignore, §1 read-before-edit, §2 StrReplace, compound Shell, log sentinels).
3. **Classify** (pick one primary; note secondary if needed):
   - **Confirmed violation** — behavior contradicts an explicit MUST/NEVER or the quick-reference table.
   - **Doc gap** — behavior is wrong, but the guide/rule does not state the expectation clearly enough for a typical agent.
   - **Ambiguous** — evidence insufficient or context-dependent (e.g. legitimate `pwsh -NoProfile -File` script vs forbidden reinvention).
   - **False positive / allowed path** — behavior matches a documented exception (e.g. catalog script, sentinel `StrReplace` pattern, temp `.ps1` last resort).
   - **Spec conflict** — rule says X, guide says Y; treat as **high priority** fix in the same pass.
4. **Decide corrections** (only when justified):
   - **Guide** — clarify edge cases, add one example, fix contradictions; keep concise (token budget).
   - **Rule** — only if alwaysApply decision tree is missing a high-leverage line the violation proves necessary; avoid large rule bloat.
   - **Related doc** — e.g. [cursor-command-allowlist.md](../../docs/agent/cursor-command-allowlist.md), [agent-file-operations/SKILL.md](../agent-file-operations/SKILL.md), [logging/SKILL.md](../logging/SKILL.md) for sentinel rows.
5. **If editing docs:** follow normal config hygiene (PCB if multi-file substantive change, CL+, backlog when tied to a story, `Invoke-FrameworkManifestValidate.ps1` if manifest touched). **If not editing:** still complete the user report.

## Known recurrence classes

Patterns observed ≥1 time across sessions; map evidence here before classifying. New classes are added as the violation log accumulates them.

| Class | Evidence shape | Canonical recipe / fix |
|-------|----------------|------------------------|
| **Stale post-rename preview cache** (binary assets) — agent reasoned about content from the Read-tool image preview *after* a rename (in-session **or** external between sessions); preview cache is path-keyed and may serve stale bytes | `Read` of an image returns content that contradicts on-disk reality after rename / external move; multiple wrong-direction renames; user clarification "I renamed outside Cursor" | [agent-file-operations.mdc § Content identity](../../rules/agent-file-operations.mdc) (hash-first recipe) + [agent-file-operations SKILL § Content identity (binary assets)](../agent-file-operations/SKILL.md) (PS7 snippet, trust order, do-not list); seed example: AL `2026-05-11 08:12:29` (issue-22 screenshot loop) |
| **Compound bare Shell** (`;`/`&&`/`if`/`$LASTEXITCODE` as one bare line) — tokenization breaks allowlist prefix matching | `git mv … && git mv …` on one line; `Get-Date -UFormat …; if (-not (Test-Path …)) { New-Item … }`; `where.exe …; if ($LASTEXITCODE -ne 0) { … }` | [agent-file-operations.mdc § NEVER + decision tree §4](../../rules/agent-file-operations.mdc) — wrap in one `pwsh -NoProfile -Command "…"` or split into one Shell call per allowlisted prefix |
| **Assignment-first bare Shell** — first token is `$var` / `$env:Path`, not a cmdlet | `$x = Get-Content …; …`; `$content = Get-Content -Raw … ; if (…)` | [agent-file-operations.mdc § NEVER](../../rules/agent-file-operations.mdc) — wrap in one `pwsh -NoProfile -Command` (escape `` `$ ``) or rewrite to lead with the cmdlet |

## Mandatory user-facing report

End the turn (or dedicated message) with this structure so the user always sees the outcome:

```markdown
## File-ops report triage

**Classification:** …
**Mapped to guide:** … (section refs)
**Verdict:** … (1–3 sentences)
**Recommended actions:**
- For the user: …
- For docs/rules (if any): …
**Changes made:** … (files) or *None — documentation already clear.*
```

Use neutral, precise language. If the user’s report is wrong, explain why without dismissing the concern.

## Logging

### 1. Dedicated triage log — ALWAYS (every triage, even read-only)

Write to `logs/file-ops-violation-log.md` in two steps (both use `StrReplace`):

**a) Increment the count**

1. `Read` `logs/file-ops-violation-log.md` — note the current count for the primary classification and the current Total.
2. `StrReplace` the count row, e.g. `| Confirmed violation | 3 |` → `| Confirmed violation | 4 |`.
3. `StrReplace` the Total row, e.g. `| **Total** | **3** |` → `| **Total** | **4** |`.

Each classification row is unique (label is unique within the table) so StrReplace works without a sentinel.

**b) Prepend a triage entry** — use the sentinel `<!-- FOPS-VIOLATION+ -->` per [logging/SKILL.md](../logging/SKILL.md) insertion method.

Entry columns: `| When | Evidence | Classification | Guide section(s) | Verdict | Changes |`

- **When**: `YYYY-MM-DD HH:mm:ss` local time.
- **Evidence**: ≤15 words describing the reported behavior (e.g. `"agent ran Get-Content path then Set-Content"`, `"transcript ref: <uuid>"`).
- **Classification**: one of the five categories from the Analysis workflow.
- **Guide section(s)**: section refs (e.g. `§1 read-before-edit`, `§2 StrReplace`).
- **Verdict**: 1 sentence.
- **Changes**: paths of edited files, or `None`.

### 2. Standard AL and chat log — after substantive triage

If you edited any file (guide, rule, or triage log): append **AL** per [logging/SKILL.md](../logging/SKILL.md) (`Action: file-ops triage`, `What` includes classification + paths). If the conversation is primarily about `.cursor` config, append a **chat log** row to `logs/cursor-chat-log.md`.

## Escalation

- Repeated **spec conflicts** between rule and guide: file or update a **CFG** backlog story under *Agent file-operations compliance* and link the plan if one exists.
- Suspected **allowlist** or **sandbox** product issue: separate from this skill; point to `docs/agent/cursor-command-allowlist.md` and IDE theme stories as appropriate.

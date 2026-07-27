---
description: >-
  PowerShell and Shell patterns for template/mirror directory audits, path-safe
  scans, machine-readable output, and allowlist-safe Windows toolchain probes.
  Use when comparing trees (for example templates vs student), reporting missing files,
  scripting bulk inventory where Read / Glob are wrong tools, or checking g++ /
  qmake / cppcheck without Run prompts. Do not use ad hoc `python -c` for the same audits
  (allowlist + correctness). Complements rules/agent-file-operations.mdc.
framework:
  id: agent-file-operations-skill
  kind: skill
  scope: operations
  topic: governance
  depends_on: [agent-file-operations, framework-overview]
  triggers: [template mirror audit, path-safe shell]
  status: active
---

# Agent file operations (Shell audits)

**Rule (short):** [agent-file-operations.mdc](../../rules/agent-file-operations.mdc) — tool substitution and NEVER list.

**This skill:** how to write **maintainable, agent-friendly** PowerShell when the task is **directory diff / inventory** (not editing file contents). Course sync semantics (week scope, overwrite consent) stay in [version-management SKILL](../version-management/SKILL.md).

## When this applies

- Compare **source tree** vs **mirror tree** (for example `templates/` → `student/`).
- List paths **missing** on one side; optional **existing** list for summaries.
- Filter by **convention** (for example only week folders `^\d{2}$` with numeric `>= StartWeek`).

**Prefer** `Glob` / `Grep` / `Read` when the question is names or content inside the workspace without a second root — this skill is for **paired roots** and large walks.

**Do not** reach for **`cd … && python -c "…os.walk…"`** here: `python` is often **absent** from `terminalAllowlist`, **`&&`** is a compound prompt trap, and one-liners often mishandle Windows separators (for example `replace('\\\\','/')` does **not** turn `\` into `/`). Stay on **PS7** + this skill.

## Location and parameters

1. **Do not** bake `Set-Location "C:\Users\…\Course"` into ad hoc one-liners for agents — paths differ across machines.
2. Use Shell **`working_directory`** set to the **opened project root** when the script assumes relative `templates` / `student`.
3. For reusable snippets, use a **`param` block** and explicit roots:

```powershell
param(
    [Parameter(Mandatory)]
    [string] $CourseRoot,
    [int] $StartWeek = 5,
    [string] $SrcRoot = 'templates',
    [string] $DstRoot = 'student'
)
Push-Location -LiteralPath $CourseRoot
try {
    # ...
} finally {
    Pop-Location
}
```

## Path canonicalization and relative paths

**Anti-pattern:** calling `Resolve-Path $srcRoot` inside `ForEach-Object` over every file.

**Do:** resolve once, then compute relatives with the API (PS 7 / .NET):

```powershell
$base = (Resolve-Path -LiteralPath $SrcRoot).Path
$rel = [System.IO.Path]::GetRelativePath($base, $file.FullName).Replace('\', '/')
```

**Why:** avoids off-by-one after `TrimEnd('\')`, works when `$base` length does not match naive substring assumptions, and normalizes separators for stable keys.

Use **`-LiteralPath`** with `Test-Path`, `Join-Path`, and `Get-ChildItem` when names may be odd.

## Validation

Before walking:

- Fail or report clearly if `$SrcRoot` is missing.
- Decide whether a missing `$DstRoot` means “everything missing” or an error; document that in the emitted object.

If week directories are expected but **no** folder matches `^\d{2}$` after filtering, return **`weekDirsFound: 0`** (or similar) in JSON — do not silently print `missing: 0` without context.

## Output contract for agents

Humans may read console text; **agents** should get **structured output**:

```powershell
[pscustomobject]@{
    courseRoot     = $CourseRoot
    srcRoot        = $SrcRoot
    dstRoot        = $DstRoot
    startWeek      = $StartWeek
    missingCount   = $missing.Count
    existingCount  = $existing.Count
    missing        = @($missing | Sort-Object)
    existingSample = @($existing | Sort-Object | Select-Object -First 20)
} | ConvertTo-Json -Depth 6
```

- Use **`-Depth`** high enough for arrays of strings.
- For very large trees, **omit** full `existing` or cap with **sample + total count**.

## Week-scoped scans (course repos)

When `$SrcRoot` has immediate children `01`…`14`:

- Filter directories where **name matches** `^\d{2}$` **and** `[int]$name -ge $StartWeek`.
- **Start week:** from user message, project config, or explicit ask — same principle as version-management sync (**do not guess**).

## Shell wrapping and allowlist

- **Compound** scripts (`;`, `if`, `$LASTEXITCODE`, loops): one **`pwsh -NoProfile -Command "…"`** or **`pwsh -File script.ps1`**, per [agent-file-operations.mdc](../../rules/agent-file-operations.mdc).
- Escape **`$`** for nested hosts: `` `$var ``.
- Heavy **`New-Object`** / long logic: **temp `.ps1` + `pwsh -File`** — last resort, but legitimate for audit scripts.
- **File size:** `Get-Item -LiteralPath "…" | Select-Object -ExpandProperty Length` (allowlisted prefix `Get-Item`). Avoid `(Get-Item "…").Length` — leading `(` breaks prefix match → **Run** prompt. [cursor-command-allowlist.md §1](../../docs/agent/cursor-command-allowlist.md) (*parenthesized expression problem*).

### Allowlist gaps — Windows toolchain probes

Read-only checks (`where.exe`, **`cd … && …`**, **`cd … && python -c …`**, **`& 'C:\msys64\ucrt64\bin\qmake.exe' -version`**) often **prompt** because the command prefix is not on `terminalAllowlist`, or because **compound** lines are tokenized per segment. Canonical table, mitigations, and **Ask every time** caveat → [cursor-command-allowlist.md §1 — *Known gaps — Windows toolchain discovery (agents)*](../../docs/agent/cursor-command-allowlist.md).

**Shortcuts:**

| Instead of | Prefer |
|------------|--------|
| `(Get-Item "path").Length` | **`Get-Item -LiteralPath "path" \| Select-Object -ExpandProperty Length`** |
| `where.exe g++` / `where.exe qmake` / `where.exe cppcheck` / `where.exe clang-tidy` | **`Get-Command g++, qmake, cppcheck, clang-tidy -ErrorAction SilentlyContinue`** (allowlisted prefix); avoid **`where.exe …; if ($LASTEXITCODE -ne 0) { … }`** as a bare Shell line (compound + non-allowlisted `where.exe`) |
| `cd $root && …` | Shell **`working_directory`** + command starting with allowlisted tool/cmdlet |
| `& 'C:\…\qmake.exe' -version` (bare) | **`pwsh -NoProfile -Command "& 'C:\…\qmake.exe' -version 2>&1"`** or a **narrow** full-path row in `permissions.json` per §3 tier protocol in the same doc |
| `cd $root && python -c "…os.walk…"` (templates vs student) | **`working_directory`** + **`pwsh -File`** audit `.ps1` or **`pwsh -NoProfile -Command`** using § Path canonicalization + **`ConvertTo-Json`** |

Rule table row → [agent-file-operations.mdc](../../rules/agent-file-operations.mdc) (*Before using Shell*).

## Content identity (binary assets)

**Trigger:** rename / swap / path change of binary assets (PNG / PDF / audio / video / large data) where prior context (transcripts, forum posts, README references) asserts a specific content-at-path relationship. Rule: [agent-file-operations.mdc § Content identity](../../rules/agent-file-operations.mdc).

**Why hash, not path:** Cursor's Read tool keeps a **path-keyed image-preview cache**. An external rename outside Cursor (file-watcher blind spot) or in-session rename cycles can serve **stale** content for the same path on subsequent Read calls. Treating the **SHA-256 hash** as the identity primitive removes the path's role as a content identifier — renames become bookkeeping.

**Recipe (PS7):**

```powershell
Get-ChildItem -LiteralPath 'forum/cursor/issues/Screenshots' -File |
    Get-FileHash -Algorithm SHA256 |
    Select-Object @{N='hash';E={$_.Hash}}, @{N='path';E={$_.Path}} |
    ConvertTo-Json -Depth 4
```

Or use the catalog script (Tier 1, JSON output, empty-folder safe):

```powershell
pwsh -NoProfile -File scripts/Invoke-AssetHashAudit.ps1 -RootPath 'forum/cursor/issues/Screenshots'
```

Then in chat, write the binding once:

```text
hash↔name (issue-22 attachments, 2026-05-11):
  3b2f…c1   → Agent_write_arguments_error.png   (chat empty-args, 46 KB)
  a7e0…99   → Agent_write_network_error.png     (toast, 14 KB)
  …
```

Subsequent renames are hash-bookkeeping: the chat-recorded hash continues to identify the asset regardless of filename.

**Anti-pattern (named):** *stale post-rename preview cache* — read → rename → re-read → infer content from the second preview. Recurrence example: AL `2026-05-11 08:12:29` (issue-22 screenshot loop, three rename cycles before Discourse-rendered dimensions broke the loop).

**Do not:**

- Re-Read the asset for content verification within the same task after a rename — Cursor's preview cache may not yet reflect the on-disk bytes.
- Trust transcript context across sessions when an asset has been touched externally — apply the cross-session asset check first (rule § Reminders).
- Use file size alone as identity — different content can have the same byte length; size is a useful **secondary** signal, hash is primary.

**Trust order when sources disagree:**

1. SHA-256 hash of the on-disk file (PS7 `Get-FileHash`).
2. Third-party rendered properties (for example Discourse-served image width/height, server `Content-Length`).
3. Local file size + `LastWriteTime`.
4. Read-tool image preview — last (cache may be stale).

Triage class for this anti-pattern: [agent-file-operations-violation-triage SKILL](../agent-file-operations-violation-triage/SKILL.md).

### After user-requested PDF downloads

Fetch stays **user-gated** (`curl.exe` / browser) — do **not** ask for a catalog download wrapper. Validate locally: `Invoke-FileMagicProbe.ps1` (preferred — `kindGuess` + hash) and/or `Invoke-AssetHashAudit.ps1` / `Format-Hex -Count 8` (expect `%PDF` = `25 50 44 46`). **Never** `Get-Content -Encoding Byte` or `[BitConverter]::…` on PS7. Full split (fetch / validate / ingest) + Osakkeet/Terveys notes: [agent-file-operations-guide.md § External PDF download + validation](../../docs/agent/agent-file-operations-guide.md#external-pdf-download-validation).

## Cross-references

- Tool choice and NEVER list → [agent-file-operations.mdc](../../rules/agent-file-operations.mdc)
- Long-form edge cases → [agent-file-operations-guide.md](../../docs/agent/agent-file-operations-guide.md)
- Allowlist prefix matching, compound commands, toolchain + **`python -c`** gaps → [cursor-command-allowlist.md](../../docs/agent/cursor-command-allowlist.md) (§1 *Known gaps — Windows toolchain discovery*)
- Git sync, overwrite prompts, week policy → [version-management SKILL](../version-management/SKILL.md)

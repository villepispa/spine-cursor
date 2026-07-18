---
framework:
  id: framework-contributing
  kind: guide
  scope: core
  topic: governance
  depends_on: [framework-overview]
  triggers: [adding a member, retiring a member, renaming a member]
  status: active
---

# Framework contributing — add, retire, rename

> **Public subset (v0.1).** Checklist for keeping overview + manifest in sync
> in this repository. Private labs may add tooling paths not present here.

**Audience:** Anyone (human or agent) proposing to change the framework's
member list. **Scope:** Mechanical checklist for
[framework-overview.md](framework-overview.md) and
[framework-manifest.yml](../framework-manifest.yml).

If you are only editing the **body** of an existing member, this checklist
does **not** apply — treat it as a normal documentation edit and record it in
the repository CHANGELOG when substantive.

---

## 1. Decide first: framework member or workspace asset?

A new doc joins the framework only if **all three** apply:

1. It addresses one of the **declared concerns** in
   [framework-overview.md § 3.1](framework-overview.md) (or proposes a new
   concern; see § 5 below).
2. It will be **referenced by other framework members** — not a leaf.
3. It can articulate a **token-cost case** (it lowers per-turn cost by
   centralizing what would otherwise be repeated, or it routes agents to the
   right place faster).

If any condition fails, ship the doc as a **workspace asset** (not listed in
the manifest, not required to carry framework front-matter).

---

## 1.5 Framework metadata in rules, skills, and agents

When the manifest lists `kind: rule`, `kind: skill`, or `kind: agent`:

- **MUST:** keep Cursor-native front-matter keys intact.
- **MUST:** add a `framework:` mapping (same fields as
  [framework-overview.md § 6](framework-overview.md)) in the **same** opening
  YAML fence — never a second fence.
- **`id`:** stable kebab-case; **MUST** equal the manifest row `id`.

---

## 1.6 Manifest `size_lines` (soft budget)

`size_lines` is a **navigation hint**, not a hard CI gate. Prefer total line
count (including blanks) from `@(Get-Content -LiteralPath $path).Count` in
PowerShell 7. Do **not** use `Measure-Object -Line` (undercounts blank lines).

---

## 2. Add a member — checklist

| # | Step | File touched |
|---|------|--------------|
| 1 | Choose **scope** (`core` / `bridge` / `adaptation` / `operations`). | — |
| 2 | Choose **kind** (`principles`, `method`, `reference`, `rule`, `skill`, `agent`, `script`, `guide`). | — |
| 3 | Pick a stable kebab-case **`id`**. | — |
| 4 | Write the file body. Prefer principles ≤ 220 lines; methods ≤ 120. | new file |
| 5 | Add YAML front-matter per [overview § 6](framework-overview.md). | new file |
| 6 | Append a **manifest** entry with all required fields. | `docs/framework-manifest.yml` |
| 7 | Update the **member list** in [overview § 7](framework-overview.md). | `docs/governance/framework-overview.md` |
| 8 | Update the **grid** in [overview § 3.3](framework-overview.md) if needed. | same |
| 9 | Add inbound links from at least **one** existing member. | varies |
| 10 | Update root **README** with a one-line description. | `README.md` |
| 11 | Append one **CHANGELOG** row. | `CHANGELOG.md` |

Framework changes that touch ≥ 3 files should use a written plan before
implementation.

---

## 3. Retire (deprecate) a member — checklist

Never delete a framework member's file. History matters.

1. Identify the **successor** (if any).
2. Set `status: deprecated` on the old member; add a deprecation banner.
3. Add `supersedes: <old-id>` on the successor.
4. Update the **manifest** status; do not remove the entry.
5. Move the overview § 7 entry to a Deprecated subsection.
6. Rewrite inbound links from **active** members to the successor.
7. CHANGELOG row noting deprecation and successor.

---

## 4. Rename a member — checklist

1. Confirm no `id` collision in the manifest.
2. Move / rename the file.
3. Update front-matter `id` and manifest `id` / `path`.
4. Update **all inbound links** (search for old `id` and old path).
5. Skip historical CHANGELOG rows already written.
6. CHANGELOG row noting the rename.

---

## 5. Add a new concern (axis row)

1. Justify why an existing concern cannot host the topic.
2. Update overview § 3.1 and § 3.3.
3. Land the first member in the new row in the **same** change set.

---

## 6. What never changes without justification

- The **front-matter schema**.
- The four **scope** values.
- The **status** taxonomy ([overview § 4](framework-overview.md)).

Schema migrations need an explicit plan and CHANGELOG entry.

---

## 7. See also

- [framework-overview.md](framework-overview.md)
- [framework-manifest.yml](../framework-manifest.yml)
- [ai-governance-principles.md](../principles/ai-governance-principles.md)

---

Change history: [CHANGELOG.md](../../CHANGELOG.md)

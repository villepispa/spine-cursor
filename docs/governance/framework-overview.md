---
framework:
  id: framework-overview
  kind: guide
  scope: core
  topic: governance
  depends_on: []
  triggers: [adding a new framework member, onboarding a new agent, framework audit]
  status: active
---

# Framework overview — Spine

> **Public subset (v0.1).** This file is a redacted export for external readers.
> The private lab may carry a larger member inventory; this repo lists only what
> ships here. Broken private-path links are intentionally omitted.

**Audience:** Humans and agents adopting Spine. **Scope:** The meta-layer that
names the documentation framework, declares its axes, enumerates members in
this public tree, and states the rules for adding, retiring, and bridging
members.

Cross-references: [framework-manifest.yml](../framework-manifest.yml),
[framework-contributing.md](framework-contributing.md),
[ai-governance-principles.md](../principles/ai-governance-principles.md).

---

## 1. Purpose

The framework is an **AI-agent-friendly software development practice
framework**. It is *not* a single methodology, *not* a single language stack,
and *not* a single IDE binding. Members are reusable principles, methods,
references, and operations docs that can be:

- **Adapted** to any project, language, or course.
- **Extended** with new concerns (axes) or scopes.
- **Audited** — every member is enumerated in
  [framework-manifest.yml](../framework-manifest.yml).

**What the framework is not:**

- A textbook (pointers to canonical sources only).
- A linter or runtime tool (documentation + conventions; tools live elsewhere).
- A course policy (courses are **adaptation** targets, not the framework).

---

## 2. Name and identity

**Working name (v1): Spine.** Single-syllable, evocative of structural
support, language- and IDE-neutral.

The framework's **identity** rests on **three contracts**, not the name:

1. Every member declares its **scope** and **topic** (front-matter).
2. Every member is listed in the **manifest**.
3. Every change to membership follows the **contributing checklist**.

A member that lacks any of the three is workspace material, not framework
material.

---

## 3. Axes

Members live at the intersection of two axes.

### 3.1 Concern (rows)

Concerns are the *topics* the framework speaks to. Core public concerns include
at least:

- **governance** — membership, audit honesty, agent accountability
- **security** — secure-by-default agent and code practice (expanded later)
- **change-management** — plans, logs, reversibility (expanded later)

### 3.2 Scope (columns)

| Scope | Meaning |
|-------|---------|
| **core** | Universal; safe to adapt anywhere |
| **bridge** | Language / paradigm specific |
| **adaptation** | Workspace / course / product binding |
| **operations** | Scripts, hooks, runbooks |

### 3.3 Public occupancy (v0.1)

| Concern | Principles / guides in this repo |
|---------|----------------------------------|
| governance | `framework-overview`, `framework-contributing`, `ai-governance-principles` |

Further cells ship in later public waves.

---

## 4. Status taxonomy

| Status | Meaning |
|--------|---------|
| `active` | Current contract |
| `bridge` | Cross-cutting helper |
| `adaptation` | Bound to a specific environment |
| `deprecated` | Kept for history; successor named via `supersedes` |
| `experimental` | May change without a migration plan |

---

## 5. Member kinds

`principle` · `method` · `reference` · `rule` · `skill` · `agent` · `script` ·
`guide`

---

## 6. Front-matter header spec

Every framework member markdown file opens with a YAML fence containing a
`framework:` mapping:

| Field | Required | Notes |
|-------|----------|-------|
| `id` | yes | Stable kebab-case; matches manifest |
| `kind` | yes | See § 5 |
| `scope` | yes | See § 3.2 |
| `topic` | yes | Concern from § 3.1 |
| `depends_on` | yes | List of other member `id`s (may be empty) |
| `triggers` | yes | Short phrases for when to consult |
| `status` | yes | See § 4 |

Rules / skills / agents that are also framework members keep Cursor-native
keys (`description`, `globs`, …) and add `framework:` in the **same** fence.

---

## 7. Members in this public repository

### Core

- [framework-overview.md](framework-overview.md) (this file)
- [framework-contributing.md](framework-contributing.md)
- [ai-governance-principles.md](../principles/ai-governance-principles.md)
- [framework-manifest.yml](../framework-manifest.yml)

Private labs may list dozens more members; they are **not** part of this export.

---

## 8. Inclusion and deprecation policy (summary)

Full checklist: [framework-contributing.md](framework-contributing.md).

- **Add** only when content does not already live in an existing member.
- **Retire** with `status: deprecated` and `supersedes` on the successor —
  never delete history.
- **Rename** `id` + path + inbound links in one change set; record in CHANGELOG.

---

## 9. Out of scope (v0.1 public)

- Full private workspace mirror
- Live machine allowlists and MCP tokens
- Marketplace plugin bundles (later phase)
- Per-language example supplements and cross-IDE portability maps

---

## 10. See also

- [framework-contributing.md](framework-contributing.md)
- [ai-governance-principles.md](../principles/ai-governance-principles.md)
- [framework-manifest.yml](../framework-manifest.yml)
- Repository [README.md](../../README.md)

---

Change history: [CHANGELOG.md](../../CHANGELOG.md)

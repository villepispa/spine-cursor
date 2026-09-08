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

# Framework overview — Spine Framework

> **Public subset (catalog `0.1`).** This file is a redacted export for
> external readers. A private lab may carry a larger member inventory; this
> repo lists only what ships here. Broken private-path links are omitted on
> purpose.

**Audience:** Humans and agents adopting Spine Framework. **Scope:** The
meta-layer that names the documentation framework, declares its axes,
enumerates members in this public tree, and states the rules for adding,
retiring, and bridging members.

Cross-references: [framework-manifest.yml](../framework-manifest.yml),
[framework-contributing.md](framework-contributing.md),
[practice-framework.md](practice-framework.md),
[ai-governance-principles.md](../principles/ai-governance-principles.md).

---

## 1. Purpose

The framework is an **AI-agent-friendly software development practice
framework**. Kind of artefact: [practice-framework.md](practice-framework.md).
It is *not* a single methodology, *not* a single language stack, and *not* a
single IDE binding. Members are reusable principles, methods, references, and
operations docs that can be:

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

**Display umbrella:** **Spine Framework**. **Short name:** Spine. Product and
plugin ids stay `spine-*`.

The framework's **membership identity** rests on **three contracts**, not the
name:

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

Scope is **not** status. Do not copy a scope token into `status:`.

### 3.3 Public occupancy (catalog `0.1`)

| Concern | Principles / guides in this repo |
|---------|----------------------------------|
| governance | `framework-overview`, `framework-contributing`, `practice-framework`, `ai-governance-principles` |

Further cells ship in later public waves.

---

## 4. Status taxonomy

Status is **lifecycle**, not a copy of **scope**. Scope
(`core` / `bridge` / `adaptation` / `operations`) stays the portable-vs-local
axis. Do not set `status` to a scope token.

| Status | Meaning |
|--------|---------|
| `active` | Current and authoritative. |
| `deprecated` | Retained for traceability; superseded by another member (`supersedes` on the successor). |
| `experimental` | New, may change shape; rules and agents should not depend on it yet. |

A `deprecated` member must keep its file (history) but should not be linked
from `active` members except as a redirect note.

---

## 5. Member kinds

`principle` · `method` · `reference` · `rule` · `skill` · `agent` · `script` ·
`guide` · `template`

`kind: template` is a copy-fill artefact (for example under `templates/`).
`kind` is a membership taxonomy; it is not 1:1 with Diátaxis reader questions
(tutorial / how-to / reference / explanation).

---

## 6. Front-matter header spec

Every framework member markdown file opens with a YAML fence containing a
`framework:` mapping:

```yaml
---
framework:
  id: <stable-kebab-case-id>
  kind: principle | method | reference | rule | skill | agent | script | guide | template
  scope: core | bridge | adaptation | operations
  topic: <single concern from § 3.1>
  depends_on: [<id>, ...]
  triggers: [<short phrase>, ...]
  status: active | deprecated | experimental
  supersedes: <id>   # optional
---
```

Rules / skills / agents that are also framework members keep Cursor-native
keys (`description`, `globs`, …) and add `framework:` in the **same** fence.

The inventory-level **`framework_version`** lives on
[framework-manifest.yml](../framework-manifest.yml) (currently `"0.1"`). It is
a **catalog revision label**, not a SemVer product release and not the same
artefact as this repository's Keep a Changelog tags (`0.4.0`, …). When to
bump: [framework-contributing.md § 1.8](framework-contributing.md).

---

## 7. Members in this public repository

### Core

- [framework-overview.md](framework-overview.md) (this file)
- [framework-contributing.md](framework-contributing.md)
- [practice-framework.md](practice-framework.md)
- [ai-governance-principles.md](../principles/ai-governance-principles.md)
- [framework-manifest.yml](../framework-manifest.yml)

Private labs may list dozens more members; they are **not** part of this
export.

---

## 8. Inclusion and deprecation policy (summary)

Full checklist: [framework-contributing.md](framework-contributing.md).

- **Add** only when content does not already live in an existing member.
- **Retire** with `status: deprecated` and `supersedes` on the successor —
  never delete history.
- **Rename** `id` + path + inbound links in one change set; record in
  CHANGELOG.

---

## 9. Out of scope (public slice)

- Full private workspace mirror
- Live machine allowlists and MCP tokens
- Per-language example supplements and cross-IDE portability maps

Marketplace plugins **do** ship in this repository (wave 1 plus
`spine-agent-probes`). They are delivery slices, not the whole framework —
[practice-framework.md](practice-framework.md).

---

## 10. See also

- [practice-framework.md](practice-framework.md)
- [framework-contributing.md](framework-contributing.md)
- [ai-governance-principles.md](../principles/ai-governance-principles.md)
- [framework-manifest.yml](../framework-manifest.yml)
- Repository [README.md](../../README.md)

---

Change history: [CHANGELOG.md](../../CHANGELOG.md)

---
framework:
  id: practice-framework
  kind: guide
  scope: core
  topic: governance
  depends_on: [framework-overview]
  triggers:
    - practice framework
    - what is Spine
    - methodology vs framework
    - process framework
    - CMMI
    - maturity
    - onboarding a new agent
  status: active
---

# Practice framework

**Audience:** humans and agents who need to say *what kind of thing*
**Spine Framework** is. **Scope:** the concept, and how a public slice
instantiates it. Membership mechanics stay in
[framework-overview.md](framework-overview.md) and
[framework-contributing.md](framework-contributing.md).

---

## 1. Definition

A **practice framework** is a structured, adoptable collection of
**practices** (repeatable ways of working), the **principles** that
justify them, and **adaptation guidance** for fitting them to a
context. Teams take what they need; they don't run a single prescribed
process from start to finish.

**Spine Framework** is an AI-agent-friendly software development
practice framework: principles, methods, governance contracts, and
adaptation guidance for teams that use coding agents (Cursor and
similar) with an honest audit trail.

**Token-cost case:** one definition here so overview § 1, README, and
onboarding don't each re-explain the kind.

---

## 2. What it isn't

| Kind | Meaning | Why Spine isn't that |
|------|---------|----------------------|
| **Methodology** | A prescribed process with fixed roles, ceremonies, and sequence (for example Scrum-as-rulebook). | Spine has no mandatory sprint ritual. Members are selected per concern and scope. |
| **Process framework** | Staged process areas and capability levels (for example CMMI, ISO/IEC 12207). | Spine inventories *practices and contracts*, not maturity stages. |
| **Software framework** | Libraries plus inversion of control (Qt, .NET, a web MVC stack). | Spine ships documentation and conventions. Tools live elsewhere (hooks, scripts, plugins). |
| **Body of knowledge** | Encyclopedic coverage of a discipline (SWEBOK, PMBOK). | Spine points at canonical sources; it doesn't replace them. |
| **Cursor plugin pack** | Marketplace extensions that add rules or skills to an IDE. | Wave-1 plugins in this repo are *delivery slices* of some Spine practices, not the framework itself. |

---

## 3. Constituent layers

| Layer | Role |
|-------|------|
| **Principles** | *Why* a practice exists |
| **Methods** | Named techniques (*how* to do a practice) |
| **Contracts** | Membership, front-matter, contributing, gates |
| **Adaptation** | Binding to a language, course, or product (`scope: bridge` / `adaptation`) |
| **Audit trail** | Honest claim of what happened (logs, changelog, optional seals) |

This public tree ships a **subset** of those layers.

---

## 4. How Spine instantiates it

1. **Concern × scope grid** — practices live at an intersection
   ([overview § 3](framework-overview.md)).
2. **Enumerated members** — every framework member in *this* repo is in
   [framework-manifest.yml](../framework-manifest.yml).
3. **Adaptation, not fork-by-default** — core stays language-neutral.
4. **Honest audit** — [ai-governance-principles.md](../principles/ai-governance-principles.md).
5. **Display name vs ids** — humans see **Spine Framework**; product ids
   stay `spine-*`.

### Nearest CMMI analogue (not a program)

Spine does **not** run CMMI maturity/capability levels or process areas.
If you know CMMI, these are the nearest *operational* analogues — for
citation, not assessment.

| CMMI-ish area | Spine analogue | Honest gap |
|---------------|----------------|------------|
| PPQA | Sphinx gate (`spine-config-gate` plugin) | Not an organizational process-quality group |
| Configuration management | Version-control tiers + changelog | Solo / small-team slice; no CM process area |
| Measurement and analysis | Agent logs + changelog + optional AI-assistance log | Qualitative audit trail, not DORA dashboards |
| Requirements / technical solution | Methods and guides by concern | Practices, not RD/TS process areas |

None of these are compliance claims. **ITIL 4** (shift from processes to
practices) is a useful *shape*; it is not used as a program here.

---

## 5. How to use it here

1. Read [framework-overview.md](framework-overview.md) for axes and
   membership.
2. Pick the **concern** and **scope**.
3. Follow `depends_on` — don't re-embed ancestor wording.
4. When adding a member, apply
   [framework-contributing.md](framework-contributing.md).

**PREFER:** cite this file when a reader asks "is Spine a methodology?",
"is this a software framework?", or "is this CMMI / a process
framework / a maturity model?"

---

## 6. Honest gap

This file describes the **kind**. The public export may lag a private lab,
omit logs and allowlists, and ship only a subset of members. That's
slice-not-lift, not a second definition of the concept.

---

## 7. See also

- [framework-overview.md](framework-overview.md)
- [framework-contributing.md](framework-contributing.md)
- [ai-governance-principles.md](../principles/ai-governance-principles.md)

---

Change history: [CHANGELOG.md](../../CHANGELOG.md)

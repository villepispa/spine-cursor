# Spine — issue register

This register tracks actionable findings and shipped product work for the
public Spine documentation tree (lane A). Prefer stable `SPC-` identifiers in
commits, release notes, and related product docs.

## Workflow

- **Status:** `Open` → `In progress` → `Resolved` → `Verified`
- **Severity:** `High`, `Medium`, or `Low` (defects); omit for shipped features
- Keep issue identifiers stable; never reuse an ID
- Add resolution evidence before changing an issue to `Verified`

## Open issues

_(None.)_

## Resolved issues (2026-08-21 — v0.3.2)

| ID | Summary | Evidence |
|----|---------|----------|
| SPC-005 | Probe envelope is evidence, not an accept; optional `criteriaHash` / `contractId` | `plugins/spine-agent-probes/` contract § 3.1 + skill; plugin `0.1.1` |

## Resolved issues (2026-07-27 — v0.3.1)

| ID | Summary | Evidence |
|----|---------|----------|
| SPC-004 | Related link to spine-automation (probe implementation sibling) | `README.md`; `CHANGELOG.md` 0.3.1 |

## Resolved issues (2026-07-27 — v0.3.0)

| ID | Summary | Evidence |
|----|---------|----------|
| SPC-003 | Ship `spine-agent-probes` plugin (AgentSummary + JSON contract) | `plugins/spine-agent-probes/`; marketplace entry; `CHANGELOG.md` 0.3.0 |

## Resolved issues (2026-07-18 — SPC-002)

| ID | Summary | Evidence |
|----|---------|----------|
| SPC-002 | Keep a Changelog + this issue register; README documentation links | `CHANGELOG.md`; `docs/issues.md`; `README.md` |

## Resolved issues (2026-07-18 — v0.1.0)

| ID | Summary | Evidence |
|----|---------|----------|
| SPC-001 | Initial public export (lane A): framework overview, contributing, AI governance principles, trimmed manifest | `docs/governance/`; `docs/principles/ai-governance-principles.md`; `docs/framework-manifest.yml`; `LICENSE`; `README.md` |

## Activity

<!-- ISSUES-ACTIVITY+ -->
- **2026-08-21 20:15:00** — Released `0.3.2` (`SPC-005` probe envelope evidence ≠ accept; plugin `0.1.1`).
- **2026-08-20 22:10:00** — Resolved `SPC-005` (probe envelope evidence ≠ accept; optional `criteriaHash` / `contractId`; plugin `0.1.1`).
- **2026-07-27 11:24:00** — Released `0.3.1` (`SPC-004` Related → spine-automation).
- **2026-07-27 10:34:00** — Resolved `SPC-004` (README Related → spine-automation).
- **2026-07-27 09:28:00** — Resolved `SPC-003` for release `0.3.0` (`spine-agent-probes` plugin + marketplace).
- **2026-07-27 09:14:00** — Opened `SPC-003`: `spine-agent-probes` plugin (contract docs + skill + marketplace entry).
- **2026-07-18 23:16:00** — Aligned `framework_version` to `0.1.0`; CHANGELOG SemVer wording matches Keep a Changelog policy.
- **2026-07-18 23:07:00** — Added `SPC-002` (changelog + issue register harmonization). Backfilled `SPC-001` into Keep a Changelog release `0.1.0`.
- **2026-07-18** — Resolved `SPC-001` (Phase A public docs tree); repo hosted at [villepispa/spine-cursor](https://github.com/villepispa/spine-cursor).

# Changelog

All notable changes to this public Spine Framework documentation tree are recorded here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning aligns with [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- **Family marketplace mark (OSS-016)** — canonical teal `assets/logo.svg`
  copied into each plugin `assets/` and referenced as `logo` in the four
  `plugin.json` manifests and marketplace entries. Raster companion
  `assets/logo.png` regenerated from that SVG (2026-08-30); plugin folders
  ship SVG only. Unused white alts and the prior AI raster moved to
  `_archive/2026-08-30_logo-raster-from-svg/` (gitignored). PCB batch
  `2026-08-29_spine-marketplace-logo`. Ville chose teal over the white
  log-row variant (2026-08-29).

### Changed

- **Family mark (OSS-016)** — Redesigned for **Spine Framework**: inset teal
  frame (framework) and a left audit gutter into four practice layers. Same
  navy/teal tile; not staggered vertebrae. Canonical `assets/logo.svg` copied
  to each plugin. Intrinsic 512×512. Raster `assets/logo.png` regenerated
  from that SVG (2026-08-31; SHA-256 `E24D3830…`). Plugin folders ship SVG
  only.
- **Public umbrella (OSS-026 / OSS-016)** — Display name is **Spine Framework**
  (README H1, marketplace description, issue register, framework overview,
  plugin README one-liners). Product and plugin ids stay `spine-*`. Not a
  rename of GitHub `spine-cursor`.
- **README mark size** — GitHub README now uses `<img width="128" height="128">`
  so the SVG does not fill the article column. `logo.svg` has intrinsic
  `width`/`height` 512 (viewBox was already 512).
- **Marketplace prep (OSS-016)** — Root README documents user vs project vs
  team install scope, wave-2 (not in this tag), and the honest gap vs a private
  Spine lab. `marketplace.json` `metadata.version` aligned to **0.3.2**. Skill
  `name` frontmatter added on three `SKILL.md` files (file-ops, logging,
  agent-summary-probe-contract) for Marketplace review.

## [0.3.2] — 2026-08-21

### Added

- **Probe envelope evidence ≠ receipt (`SPC-005`)** — `spine-agent-probes`
  contract § 3.1: `-AgentSummary` / JSON envelope is evidence, not an accept;
  optional `criteriaHash` / `contractId`. Plugin `0.1.1`.

## [0.3.1] — 2026-07-27

### Changed

- **Related** — README links [spine-automation](https://github.com/villepispa/spine-automation)
  as the PowerShell implementation sibling of `spine-agent-probes` (`SPC-004`).

## [0.3.0] — 2026-07-27

### Added

- **`spine-agent-probes` plugin (`0.1.0`, SPC-003)** — public
  `-AgentSummary` / `-Json` probe contract
  (`plugins/spine-agent-probes/docs/agent-summary-probe-contract.md`) plus skill;
  registered in marketplace after wave-1 trio
- Product issue register tracked in-repo (`docs/issues.md`, including `SPC-003`)

### Changed

- **Vale / Microsoft Writing Style Guide** — cleared non-`Contractions` error-level
  alerts on lane-A docs and wave-1 plugin prose (`e.g.` → `for example`, Avoid
  `backbone` → `support`, Quotes / Plurals / HeadingColons / Spacing / GenderBias
  rewords). `Microsoft.Contractions` left as intentional debt (`738817c`).

## [0.2.0] — 2026-07-26

### Added

- Wave 1 marketplace plugins (`0.1.0` each): `spine-agent-file-operations`,
  `spine-logging-governance`, `spine-config-gate`
- Root `.cursor-plugin/marketplace.json` (`pluginRoot: plugins`)
- Per-plugin `plugin.json`, README, empty AL/AAL templates, `setup-buddy-lite`
  agent
- Product issue register (`docs/issues.md`, `SPC-` IDs) and README documentation
  links (`SPC-002`)

### Changed

- **Member kind `principles` → `principle`** — singular enum aligned with
  `method` / `rule` / … in overview § 5, contributing checklist, manifest, and
  `ai-governance-principles` frontmatter (private source tree unchanged)

## [0.1.0] — 2026-07-18

### Added

- Initial public export (lane A): framework overview, contributing checklist,
  AI governance principles, trimmed `framework-manifest.yml` (`SPC-001`)
- MIT `LICENSE`, root `README.md`, public `.gitignore` (default-deny for
  machine-local / private adaptation paths)
- Redaction gate: § 3 P0 findings = 0 before first push

### Changed

- **`framework_version`** — set to `0.1.0` in `docs/framework-manifest.yml` (was
  `0.1`) so the manifest matches this SemVer release.

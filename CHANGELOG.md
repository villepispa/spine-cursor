# Changelog

All notable changes to this public Spine documentation tree are recorded here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning aligns with [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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

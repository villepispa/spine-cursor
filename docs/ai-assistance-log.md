# AI assistance log — Spine Framework (spine-cursor)

Human-readable disclosure trail for substantive AI-assisted work on the public
Spine Framework documentation + plugin marketplace repository. Complements
product `CHANGELOG.md` (release history) and the product-local
`.cursor/agent-log.md` (per-turn agent mechanics). Newest-first at `<!-- AAI+ -->`.

**Note on private lab origin:** portions of the public plugins are sliced from
a private Cursor config lab. Wave-1 extraction was AI-assisted; individual
private-source authorship is *not* AAI-tracked here.

| When | Phase / scope | Deliverable | Tool / model | Purpose | Verified by |
|------|---------------|-------------|--------------|---------|-------------|
<!-- AAI+ -->
| 2026-09-08 20:45:00 | 0.5.0 release (SPC-008/009/010) | Keep a Changelog 0.5.0; marketplace 0.5.0; issues resolved | Cursor Agent [Tier 2: cursor-grok-4.6] | Stream G1/G2 + annotated `v0.5.0` | Operator: this release |
| 2026-09-08 20:39:00 | SPC-010 accept | Named accept of lane A + wave-1 catch-up | Cursor Agent [Tier 2: cursor-grok-4.6] | Close the public-slice catch-up; wave-2 stays later | Ville (user `accept`) ≠ Pulse |
| 2026-09-08 19:56:00 | SPC-010 lane A + wave-1 catch-up | Catalog `0.1`; `practice-framework.md`; wave-1 plugin re-export; wave-2 deferred | Cursor Agent [Tier 2: cursor-grok-4.6] | Public schema + plugin slice after lab review | Pending named accept |
| 2026-09-08 18:50:00 | Review vs private lab | Chat findings only (no public-tree edits that turn) | Cursor Agent [Tier 1: cursor-grok-4.6] | Check whether public `framework-manifest.yml` and plugins still match the private membership contract | Operator: that chat |
| 2026-09-05 16:50:00 | SPC-009 plugin-template polish | Four `plugin.json` `displayName` fields; vendored `scripts/validate-template.mjs`; README Validate | Cursor Agent [Tier 2: cursor-grok-4.6] | Marketplace polish vs cursor/plugin-template; walker skips `_archive` | Pending named accept |
| 2026-09-05 16:31:31 | Product init catch-up — AAI bootstrap + markdownlint trio | This file + `.markdownlint*.json` + `.markdownlint-cli2.jsonc` + `.markdownlintignore` | Cursor Agent [Tier 2, slug: unlogged] | Per-product AAI disclosure; markdownlint trio catch-up for docs-heavy plugin marketplace repo | Pending human review |
| 2026-09-04 | SPC-008 VirusTotal release scan | `.github/workflows/virustotal-release-scan.yml` | Cursor Agent [Tier 2: cursor-grok-4.6] | Release URL scan on `release: published` | G1 `8706f76` (Stream) |
| 2026-09-02 | 0.4.0 release (SPC-006/SPC-007) | Family marketplace mark + `loom-lite` rename | Cursor Agent (see product CHANGELOG 0.4.0) | Rebrand to Spine Framework family mark; agent id rename | Operator: release |

*Rows prior to 2026-09-05 are backfilled headline references — full session-level attribution lives in `CHANGELOG.md` and the product-local `.cursor/agent-log.md`. Going forward, add one AAI row per substantive phase boundary.*

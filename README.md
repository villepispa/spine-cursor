# Spine

**Spine** is an AI-agent-friendly software development practice framework:
principles, governance contracts, and adaptation guidance for teams that use
coding agents (Cursor and similar) with an honest audit trail.

This public repository ships **lane A** (framework docs) and **lane B** Cursor
Marketplace plugins (wave 1 plus `spine-agent-probes`). Private lab material
stays private — **slice, don't lift**.

## What's in this repo (v0.3)

| Path | Role |
|------|------|
| [`docs/governance/framework-overview.md`](docs/governance/framework-overview.md) | Spine identity, axes, front-matter contract |
| [`docs/governance/framework-contributing.md`](docs/governance/framework-contributing.md) | Add / retire / rename checklist |
| [`docs/principles/ai-governance-principles.md`](docs/principles/ai-governance-principles.md) | Memory, sealing, attribution, accountability |
| [`docs/framework-manifest.yml`](docs/framework-manifest.yml) | Trimmed inventory of exported members only |
| [`docs/issues.md`](docs/issues.md) | Product issue register (`SPC-` IDs) |
| [`.cursor-plugin/marketplace.json`](.cursor-plugin/marketplace.json) | Multi-plugin marketplace manifest |
| [`plugins/spine-agent-file-operations/`](plugins/spine-agent-file-operations/) | File-ops rule + skills (`0.1.0`) |
| [`plugins/spine-agent-probes/`](plugins/spine-agent-probes/) | `-AgentSummary` / `-Json` probe contract + skill (`0.1.0`) |
| [`plugins/spine-logging-governance/`](plugins/spine-logging-governance/) | Logging rule + skill + empty log templates |
| [`plugins/spine-config-gate/`](plugins/spine-config-gate/) | Sphinx gate rule + lite agent |
| [`LICENSE`](LICENSE) | MIT |

## Local plugin install

Symlink each plugin directory into `~/.cursor/plugins/local/<name>`, then
**Reload Window**. See each plugin `README.md`.

## Documentation

- [CHANGELOG.md](CHANGELOG.md)
- [docs/issues.md](docs/issues.md)

## What is not here

- Private lab logs, plans, decision seals, or machine allowlists
- Live MCP server configs or shell allowlists (consumers keep their own)
- Full private workspace mirror

## License

MIT — see [`LICENSE`](LICENSE).

## AI assistance

Public docs were prepared with Cursor Agent assistance for redaction and
structure. Humans own every publish decision. Product changelog starts at
`v0.1.0` in this tree.

## Related

- Cursor [Plugins reference](https://cursor.com/docs/reference/plugins) (future plugin waves)
- Probe **implementation** (PowerShell module + dual-host / ShellGuard templates): [spine-automation](https://github.com/villepispa/spine-automation) — use with `plugins/spine-agent-probes`
- Sibling product: [winget-audit](https://github.com/villepispa/winget-audit)

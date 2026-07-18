# Spine

**Spine** is an AI-agent-friendly software development practice framework:
principles, governance contracts, and adaptation guidance for teams that use
coding agents (Cursor and similar) with an honest audit trail.

This public repository is **Phase A** — a redacted **framework documentation**
subset (lane A). Marketplace plugins (file-ops, logging, config gate) ship in a
later phase.

## What's in this repo (v0.1)

| Path | Role |
|------|------|
| [`docs/governance/framework-overview.md`](docs/governance/framework-overview.md) | Spine identity, axes, front-matter contract |
| [`docs/governance/framework-contributing.md`](docs/governance/framework-contributing.md) | Add / retire / rename checklist |
| [`docs/principles/ai-governance-principles.md`](docs/principles/ai-governance-principles.md) | Memory, sealing, attribution, accountability |
| [`docs/framework-manifest.yml`](docs/framework-manifest.yml) | Trimmed inventory of exported members only |
| [`LICENSE`](LICENSE) | MIT |

## What is not here

- Private lab logs, plans, decision seals, or machine allowlists
- Live MCP server configs or shell allowlists (consumers keep their own)
- Full private workspace mirror — **slice, don't lift**
- Cursor Marketplace plugins (coming in a follow-up release)

## License

MIT — see [`LICENSE`](LICENSE).

## AI assistance

Public docs were prepared with Cursor Agent assistance for redaction and
structure. Humans own every publish decision. Product changelog starts at
`v0.1.0` in this tree.

## Related

- Cursor [Plugins reference](https://cursor.com/docs/reference/plugins) (future plugin waves)
- Sibling product: [winget-audit](https://github.com/villepispa/winget-audit)

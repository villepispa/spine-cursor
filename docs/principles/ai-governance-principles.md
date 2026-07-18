---
framework:
  id: ai-governance-principles
  kind: principles
  scope: core
  topic: governance
  depends_on:
    - framework-overview
  triggers:
    - agent memory question
    - audit trail design
    - exam mode integrity
    - decision sealing
    - reproducibility metadata
    - EU AI Act
    - NIST AI RMF
    - ISO IEC 42001
    - cross-workspace boundary
    - memory poisoning
  status: active
---

# AI governance principles

> **Public subset (v0.1).** Principles for governing AI-agent work with an
> honest audit substrate. Operational paths named below are **patterns**;
> consumer workspaces implement their own logs, plans, and seals.

This document is the **principles layer** for governance of AI-agent work: how
*persistent agent memory*, *decision provenance*, *attribution*,
*reproducibility*, and *human accountability* stay honest as agents grow more
capable.

**Field source.** Distinguishing *memory-state versioning* (vendor-shipped)
from *decision-level sealing* (the stronger audit question) is intentional —
see [Anthropic Claude Managed Agents memory](https://platform.claude.com/docs/en/managed-agents/memory)
and related practitioner discussion (2026).

**Scope note.** This is the *principles* layer. It is **not** a legal
compliance claim. Frameworks like the EU AI Act, NIST AI RMF, and ISO/IEC 42001
are referenced as **shape templates** (see § 10).

---

## 1. Memory inventory is mandatory

Every artefact that can carry state from one agent decision into a **later**
agent decision must be enumerated, classified by **mutability**, and tagged
with its **audit status** before it is trusted. Unenumerated memory is
invisible memory; invisible memory cannot be governed.

**Pattern:** Maintain a memory inventory table (rules, skills, plans, logs,
transcripts, hooks, permissions, MCP, product memories, chat, working tree).
Adding a new memory surface means **adding a row first**, then wiring.

**Anti-pattern:** Treating opaque product memories as "just preferences" until
one steers a high-stakes chat.

---

## 2. Memory versioning is not decision sealing

Vendor memory layers answer **"what was in memory at time T"**. Decision
sealing answers: **"this output O at time T was derived from exactly inputs I,
retrieved memories M@v, policy P@v, and model R, and any post-hoc edit to those
references breaks the seal."** The first is a **prerequisite** for the second;
it is **not** a substitute.

**Pattern:** Human-readable decision traces (agent log + changelog + plan) plus
optional cryptographic binding (hash over plan + declared scope paths).

**Anti-pattern:** Pointing at a vendor memory-version log and concluding
"auditability is solved."

---

## 3. Three attribution questions

For any **substantive** agent decision (not every line edit), the audit
substrate should answer:

1. **Which memories / rules were retrieved** to produce this output?
2. **Why were they ranked relevant** at this step?
3. **What causal weight did each carry** in the final output?

**Pattern:** Log rows capture (1) via scope/reference; (2) often inferred from
chat; (3) is commonly still a gap — state the gap honestly (§ 8).

**Anti-pattern:** Logging "memory was used" without naming *which* slice and
*why* it ranked.

---

## 4. Reproducibility metadata first-class on plans

Plans are a useful **decision unit**. Prefer optional fields for **model
slug**, **tier recommended / actual**, **rules commit**, and a **seal** path
when a decision seal was produced. Without these, a plan is *consistent* but
not *re-verifiable*.

**Anti-pattern:** "We can rerun the plan" with no record of which model produced
it, on which rules commit, at which tier.

---

## 5. Exam-mode integrity excludes persistent memory

Where assessments are **AI-free**, persistent agent memory across sessions
silently breaks that promise. Exam (or mock-exam) mode **must** suppress,
disable, or run in a profile that does not provision persistent memory.

**Anti-pattern:** "Memory is fine, we just told the agent not to use exam
content." If the surface cannot be inspected, instruction is hope; provisioning
is enforcement.

---

## 6. Cross-workspace memory boundary

A config / governance workspace and a graded / production student (or product)
workspace are **separate AI-policy domains**. Memory bleed across them is a
**boundary violation**, not a productivity feature.

**Anti-pattern:** Pasting foreign-workspace transcripts into a prompt as
"context" without surfacing the boundary explicitly.

---

## 7. Memory-poisoning resilience

Persistent memory turns transient prompt injection into **persistent** prompt
injection. Inputs that touch memory must be **validatable**, **redactable**,
and **recoverable**.

**Pattern:** Keep stable policy in reviewable version control; let opaque
surfaces hold preferences that do not change agent decisions.

**Anti-pattern:** "Memory hygiene" as an unwritten promise with no review
cadence or redaction path.

---

## 8. Honest claim, honest gap

State what the workspace **does** provide for audit, what it **partially**
provides, and what it **does not yet** provide. Compliance overclaim is a
corrosive failure mode.

| Property | Typical status in a small agent lab |
|----------|-------------------------------------|
| Append-only agent log with scope, action, why, tier | Provided (when practiced) |
| Pinned model slug on every log row | Often partial |
| Auto-approval audit trail | Provided (when practiced) |
| Substantive-edit changelog | Provided (when practiced) |
| Plan-first decision artefact | Provided (when practiced) |
| Memory inventory | Provided (when practiced) |
| Cryptographic per-decision sealing | Optional / advanced |
| Per-output causal-weight attribution | Common gap |
| Deterministic re-execution from inputs alone | Usually out of scope |
| Compliance certification | **Not claimed** — shape templates only |

**Anti-pattern:** A compliance row labeled "Yes" because infrastructure exists
when evidence for the specific output does not.

---

## 9. Human-in-the-loop accountability

"The agent decided X" never absolves the human; the human is the **accountable
party** for every committed change. Auto-approval is for **mechanical** actions
inside an explicit allowlist — the allowlist itself is a human decision.

**Anti-pattern:** Auto-approval lists that grow until the human signature is
structurally absent from substantive change.

---

## 10. Regulatory cross-walk is illustrative, not binding

| Framework / Standard | Shape it lends |
|----------------------|----------------|
| **EU AI Act, Article 13** | Decision provenance + sealed binding above memory versioning |
| **NIST AI RMF 1.0** | Inventory (Map), measurement, management |
| **ISO/IEC 42001:2023** | Documented policies, recorded decisions, periodic review |
| **UNESCO Recommendation on the Ethics of AI (2021)** | Accountability and oversight |
| **OWASP / NIS2** | Memory-poisoning resilience as a security event |

**Anti-pattern:** Quoting Article 13 as a check-mark while gaps in § 8 remain
undocumented.

---

## Framework references

| Framework / Standard | Relevance |
|----------------------|-----------|
| EU AI Act Art. 13 | Transparency / information to deployers |
| NIST AI RMF 1.0 | Govern / Map / Measure / Manage |
| ISO/IEC 42001:2023 | AI management systems |
| OWASP LLM Top 10 (LLM01, LLM03) | Injection / poisoning lineage |
| [Anthropic Managed Agents — memory](https://platform.claude.com/docs/en/managed-agents/memory) | Vendor memory-state versioning |

---

## See also

- [framework-overview.md](../governance/framework-overview.md)
- [framework-contributing.md](../governance/framework-contributing.md)
- Repository [README.md](../../README.md)

---

Change history: [CHANGELOG.md](../../CHANGELOG.md)

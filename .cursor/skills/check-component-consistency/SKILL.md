---
name: check-component-consistency
description: >-
  Audit GS++ trestle component implementations for consistency: rule references
  across CSV/markdown/JSON, repeated or contradictory prose, divergent statements
  among related controls, and wording alignment. Use when the user asks for a
  consistency check, prose/rules audit, cross-control review, wording review, or
  to validate md_components/ before or after enrich PRs.
---

# Check Component Consistency

Review one control, a related group (e.g. `DET.5.10.*`), or the whole
`COMPONENT_DEFINITION` artifact for **implementation consistency**. Report
findings; do **not** auto-fix unless the user asks.

Sibling skill [enrich-component-implementation](../enrich-component-implementation/SKILL.md)
authors content. This skill **audits** it.

## Scope

| Input | What to check |
|-------|----------------|
| Control ID(s) or `md_components/…/{id}.md` path(s) | Those files + shared rules/siblings |
| Area prefix (e.g. `DET.5`, `BER.3`) | All markdown under that folder |
| `all` / no args | Full `md_components/{COMPONENT_DEFINITION}/` (batch; summarize) |

Default artifact = `COMPONENT_DEFINITION` from `config.env`
(`gs-plus-plus-rhel-host-rhel9`).

**Sources of truth** (same as enrich):

| Concern | Source |
|---------|--------|
| Rule ↔ control map | `data/{COMPONENT_DEFINITION}.csv` |
| `Rule_Id`, params, IR description/status | `component-definitions/…/component-definition.json` |
| Prose + status display | `md_components/…/{control-id}.md` |
| `### Rules:` display | Regenerated from JSON (must match CSV) |

## Prerequisites

- Read target markdown files (prose between HTML comments and `### Rules:` /
  `### Implementation Status:`).
- Load CSV rows whose `$$Control_Id_List` intersects the scope.
- For rule/JSON checks: grep or jq the matching implemented-requirements in
  `component-definitions/{COMPONENT_DEFINITION}/component-definition.json`.
- For related-control checks: include **siblings** (same area folder and/or
  controls sharing a CSV rule row).

Do **not** invent missing controls. Skip empty/placeholder prose with a note.

## Workflow

```
- [ ] 0. Resolve scope (controls + sibling set)
- [ ] 1. Rule reference consistency
- [ ] 2. Intra-control prose: repetition
- [ ] 3. Intra-control prose: contradiction
- [ ] 4. Cross-control: related statements
- [ ] 5. Wording consistency
- [ ] 6. Emit report (fix only if user asked)
```

### Step 0 — Resolve scope

1. Resolve markdown paths under
   `md_components/{COMPONENT_DEFINITION}/**/{control-id}.md`.
2. Build **sibling set**:
   - same parent folder (e.g. all `DET.5/*.md`), and
   - any control that shares a `$$Rule_Id` row in the CSV with a scoped control.
3. For each control extract: ID, statement, guidance, prose, status, `### Rules:`
   bullets, frontmatter rule names (if present).

### Step 1 — Rule reference consistency

For each scoped control, verify:

| Check | Fail when |
|-------|-----------|
| CSV ↔ MD Rules | Control in CSV `Control_Id_List` for rule R, but R missing from `### Rules:` (or reverse) |
| CSV ↔ JSON | IR `Rule_Id` props ≠ CSV set for that control |
| Frontmatter ↔ Rules | `x-trestle-comp-def-rules` names ≠ `### Rules:` bullets |
| Prose cites rule IDs | German prose names CaC ids or `gspp_impl_*` as if they were product features |
| Dual binding | Control listed on both a CaC row **and** a `gspp_impl_*` seed |
| Seed naming | Seed id ≠ `gspp_impl_{control}` (dots→`_`, lower) or seed shared across controls |
| Orphan | Markdown/JSON IR exists but control absent from every CSV `Control_Id_List` |
| Shared-rule intent | Same rule on unrelated topic areas with no thematic link (flag for review) |

Details: [reference.md](reference.md#rule-references).

### Step 2 — Prose repetition

Within each control’s implementation prose:

- Near-duplicate sentences or clauses restating the same fact.
- Boilerplate copied twice (same doc blurb, same mechanism sentence).
- Status/guidance echoed verbatim into prose without adding host-specific detail.

Across siblings (same area or shared rules):

- Near-identical paragraphs where controls demand **different** obligations
  (copy-paste smell). Same shared mechanism sentence is OK if each control still
  states its distinct obligation.

Severity: [reference.md](reference.md#repetition).

### Step 3 — Prose contradiction

Within one control:

- Claims mechanism X is always on, then says X is optional/absent.
- Status vs prose mismatch (e.g. `implemented` while prose lists major gaps;
  `not-applicable` while prose describes on-host enforcement).
- Prose vs control statement: denies a required technical capability the
  statement asks for, without marking `alternative` / `partial` / org gap.
- Prose vs attached rules: rules imply GPG/PAM/audit check but prose claims
  “no technical lever” (or reverse).

Flag only clear conflicts, not soft nuance. See
[reference.md](reference.md#contradiction).

### Step 4 — Related controls — divergent statements

For each sibling pair/group that shares topic or rules:

1. Summarize how each describes the **shared mechanism** (e.g. DNF GPG,
   faillock, Satellite, authselect).
2. Flag when they assert incompatible facts (different defaults, different
   scope of coverage, one says “fully covered” and sibling says “not available”
   for the same subsystem).
3. Flag when related controls use **incompatible status** for the same coverage
   level without explaining the difference.
4. Do **not** require identical prose — each control should answer **its**
   statement; require compatible facts and aligned terminology.

Examples of related groups: `DET.5.10.*` (patch/source/integrity),
`BER.3.8`/`BER.3.10`/`BER.3.11` (faillock), `KONF.7.*` (logging/audit siblings).

### Step 5 — Wording consistency

Across scope (and vs enrich conventions):

| Topic | Prefer |
|-------|--------|
| Product | `RHEL` / `Red Hat Enterprise Linux` used consistently; avoid mixing `yum` for RHEL 9 when prose means DNF (unless docs/history need yum) |
| Doc footer | `Weitere Informationen:` + markdown links; no invented URLs |
| Language | German implementation prose; no English-only mechanism dumps unless quoting a config key |
| Config paths / keys | Same spelling (`gpgcheck`, `/etc/pam.d/system-auth`, …) across siblings |
| Tone | Match sibling length/register (2–5 sentences typical); no CaC jargon in prose |
| Status vocabulary | Only: `implemented`, `partial`, `planned`, `alternative`, `not-applicable` |

Report systematic wording drift (same concept, many labels) as **wording**, not
as contradiction, unless facts conflict.

### Step 6 — Report

Emit a structured report (markdown). Do **not** edit files unless the user
explicitly asks to fix findings.

```markdown
# Consistency check — {scope}

## Summary
- Controls reviewed: N
- Findings: X critical, Y warning, Z note

## Findings

### [CRITICAL|WARNING|NOTE] {short title}
- **Controls:** {ids}
- **Category:** rules | repetition | contradiction | related | wording
- **Evidence:** quote or path:line / CSV rule id
- **Why it matters:** one sentence
- **Suggested fix:** one sentence (no drive-by edits)

## Clean
- Controls with no findings (optional list)
```

Severity guide: [reference.md](reference.md#severity).

If user asks to fix: prefer one control (or one finding cluster) per follow-up;
reuse enrich skill for prose/CSV/JSON sync. Do not hand-edit `### Rules:`.

## Example invocation

```
Consistency check DET.5.10.4
```

```
Consistency check DET.5.10
```

```
Check component consistency for BER.3.8, BER.3.10, BER.3.11
```

```
Consistency check all md_components for gs-plus-plus-rhel-host-rhel9
```

## Additional resources

- [Check criteria, severity, examples](reference.md)
- [enrich-component-implementation](../enrich-component-implementation/SKILL.md)
- `data/{COMPONENT_DEFINITION}.csv`, `config.env`

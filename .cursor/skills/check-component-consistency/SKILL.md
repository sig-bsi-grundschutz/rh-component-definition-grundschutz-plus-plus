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
| `### Rules:` display | Regenerated from JSON; CaC ids must match CSV; seed-only display may lag if FM already lists seed |

## Prerequisites

- Read target markdown files (prose between HTML comments and `### Rules:` /
  `### Implementation Status:`).
- Load CSV rows whose `$$Control_Id_List` intersects the scope.
- For rule/JSON checks: grep or jq the matching implemented-requirements in
  `component-definitions/{COMPONENT_DEFINITION}/component-definition.json`.
- For related-control checks: primary = same area folder and/or shared CSV
  rule row; catalog `related` links only when a shared mechanism/theme exists
  (see Step 0).

Do **not** invent missing controls. Skip empty/placeholder prose with a note.

## Precision (anti false-positive)

Prefer **signal over volume**. Full-repo runs that drown in style/display notes
fail the skill. Details: [reference.md](reference.md#precision-anti-false-positive).

Hard rules:

1. **SoT first:** CRITICAL only for CSV ↔ JSON rule-map breaks, dual CaC+seed,
   orphan IR, or MD `### Rules:` that **advertise wrong CaC ids** (or omit CaC
   ids that CSV/JSON map). Display-only gaps are never CRITICAL.
2. **Silence = success:** Do **not** emit findings that conclude OK / compatible /
   justified / usually intentional / no action. Put those under Clean or omit.
3. **One finding per defect:** Do not double-count the same sync issue as
   separate FM + Rules + seed findings for one control.
4. **Batch (`all`):** Cluster by issue class (one finding + control list). Cap
   style/enrich-convention noise (see Step 5 / reference).
5. **Before emit:** Drop any finding whose evidence is soft nuance, path-style
   drift, or “worth a human skim” without a clear conflict.

## Workflow

```
- [ ] 0. Resolve scope (controls + comparison set)
- [ ] 1. Rule reference consistency
- [ ] 2. Intra-control prose: repetition
- [ ] 3. Intra-control prose: contradiction
- [ ] 4. Cross-control: related statements
- [ ] 5. Wording consistency
- [ ] 6. Precision filter, then emit report (fix only if user asked)
```

### Step 0 — Resolve scope

1. Resolve markdown paths under
   `md_components/{COMPONENT_DEFINITION}/**/{control-id}.md`.
2. Build **comparison set** (ordered, narrow → wide):
   - **Primary:** same parent folder (e.g. all `DET.5/*.md`).
   - **Primary:** controls sharing a `$$Rule_Id` CSV row with a scoped control.
   - **Secondary (catalog links):** only if the linked control also shares a
     mechanism/theme with the scoped control (same subsystem: faillock, GPG,
     AIDE, …). Catalog `related` alone does **not** force a finding.
3. For each control extract: ID, statement, guidance, prose, status, `### Rules:`
   bullets, frontmatter **rule names only** (see Step 1).

### Step 1 — Rule reference consistency

**Source of truth = CSV ↔ JSON.** Markdown `### Rules:` and frontmatter are
regenerated display.

| Check | Severity / when |
|-------|-----------------|
| CSV ↔ JSON `Rule_Id` | **CRITICAL** on any symmetric difference |
| Dual binding | **CRITICAL** — control on CaC row **and** `gspp_impl_*` seed |
| Orphan | **CRITICAL** — MD/JSON IR but control absent from every CSV list |
| Seed naming | **CRITICAL** — id ≠ `gspp_impl_{control}` (dots→`_`, lower) or seed shared |
| MD Rules vs CSV — **CaC** | **CRITICAL** — CaC ids in CSV/JSON missing from MD, **or** MD lists CaC ids not in CSV/JSON |
| MD Rules vs CSV — **seed-only** | **NOTE** (batch: one clustered finding) only if FM also lacks the seed; if FM `x-trestle-comp-def-rules` already lists the seed and CSV=JSON → **ignore** missing `### Rules:` heading |
| Frontmatter vs CSV | Compare **only** `x-trestle-comp-def-rules.*.name`. Never treat `x-trestle-rules-params`, `x-trestle-param-values`, or catalog param keys as rules. If MD Rules already covered the same defect → do not add a second finding |
| Prose cites rule IDs | **WARNING** — German prose names CaC / `gspp_impl_*` as product features |
| Shared-rule intent | **Default ignore.** **NOTE** only when same CaC rule is on controls with **no** shared mechanism/theme (likely accidental `Control_Id_List` append) |

Details: [reference.md](reference.md#rule-references).

### Step 2 — Prose repetition

Within each control’s implementation prose:

- Near-duplicate sentences or clauses restating the same fact.
- Boilerplate copied twice (same doc blurb, same mechanism sentence).
- Status/guidance echoed verbatim into prose without adding host-specific detail.

Across comparison-set controls:

- **WARNING** only when two controls with **different** statements share nearly
  identical prose and neither states its distinct obligation (assessor cannot
  tell them apart).
- Shared mechanism sentences (Satellite, crypto-policies, faillock) are **OK**
  when each file still answers its own statement.

Severity: [reference.md](reference.md#repetition).

### Step 3 — Prose contradiction

Within one control — flag **only clear conflicts**:

- Claims mechanism X is always on, then says X is optional/absent.
- Status vs prose mismatch (e.g. `implemented` while prose lists major gaps;
  `not-applicable` while prose describes on-host enforcement).
- Prose vs control statement: denies a required technical capability the
  statement asks for, without marking `alternative` / `partial` / org gap.
- Prose vs attached **CaC** rules: rules imply GPG/PAM/audit check but prose
  claims “no technical lever” (or reverse). Seed-only Path B: do not treat
  “no CaC check” as contradiction.

Soft nuance, incomplete coverage already reflected in `partial`/`alternative`,
and org-vs-host scope framing → **ignore**. See
[reference.md](reference.md#contradiction).

### Step 4 — Related controls — divergent statements

Compare **primary** pairs first (folder / shared rules). Use catalog-related
links only as secondary candidates when a shared mechanism exists.

1. Summarize how each describes the **shared mechanism** (e.g. DNF GPG,
   faillock, Satellite, authselect).
2. Flag when they assert **incompatible facts** about that same subsystem.
3. Flag incompatible **status for the same coverage claim** only when statements
   demand comparable on-host coverage and prose gives no reason for the split.
4. Different statements → different status/prose is **expected**. Do not flag.
5. Never emit a finding that only says the pair is compatible / OK.

Examples of primary groups: `DET.5.10.*`, `BER.3.8`/`BER.3.10`/`BER.3.11`,
`KONF.7.*` (when sharing rules or the same logging/audit mechanism).

### Step 5 — Wording consistency

Focus on assessor-facing drift that causes confusion. Enrich style nits are
secondary.

| Topic | Flag when |
|-------|-----------|
| Status vocabulary | Value outside `implemented` \| `partial` \| `planned` \| `alternative` \| `not-applicable` → **WARNING** |
| Product / DNF vs yum | Systematic “yum installs…” as primary RHEL 9 verb (paths like `/etc/yum.repos.d/` are **OK**) |
| Commands in ticks | Plain shell commands that siblings tick — only if systematic; ignore path strings |
| Config paths / keys | Same concept, conflicting spellings **that change meaning**; `system-auth` vs `/etc/pam.d/system-auth` alone → **ignore** |
| Language | English-only mechanism dumps (not identifier quotes) → **NOTE**, small scopes only |
| Doc footer | **Out of scope for `all` / full-repo runs** (enrich convention). Single-control or small group: optional one **NOTE** if prose non-empty and siblings in-scope all have `Weitere Informationen:` |

Do **not** mass-list missing doc footers. Report systematic concept-label drift
as **wording**, not contradiction, unless facts conflict.

### Step 6 — Report

Run the precision filter ([reference.md](reference.md#precision-anti-false-positive)),
then emit a structured report. Do **not** edit files unless the user explicitly
asks to fix findings.

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

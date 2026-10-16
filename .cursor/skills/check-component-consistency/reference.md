# Reference — check-component-consistency

## Severity

| Level | Use when |
|-------|----------|
| **CRITICAL** | Rule map broken (CSV/MD/JSON disagree); orphan IR; dual CaC+seed; clear factual contradiction that would mislead an assessor |
| **WARNING** | Related controls disagree on a shared mechanism; status vs prose mismatch; heavy copy-paste across distinct obligations; systematic wording drift |
| **NOTE** | Mild repetition; style/register drift; optional shared-rule review; missing doc footer when siblings have one |

When unsure between CRITICAL and WARNING → **WARNING**.

## Rule references

### Triple match (per control)

Build three sets of rule ids for control `C`:

1. **CSV** — every `$$Rule_Id` whose `$$Control_Id_List` contains `C`
2. **MD** — bullets under `### Rules:` (if heading absent → empty set; note if CSV non-empty)
3. **JSON** — `Rule_Id` props on the IR for `C` in
   `component-definitions/{COMPONENT_DEFINITION}/component-definition.json`

Also compare frontmatter `x-trestle-comp-def-rules` / `*.name` to MD when present.

Any symmetric difference → **CRITICAL** (stale regenerate, bad CSV edit, or
hand-edited Rules).

### Prose must not sell rule ids

Implementation prose is for assessors reading host behavior. Flag if prose
contains:

- CaC-style snake_case rule ids (`ensure_gpgcheck_globally_activated`, …)
- `gspp_impl_*` seeds cited as checks or products

Config keys (`gpgcheck`), package names, and paths are fine.

### Narrative seeds

| Expectation | Fail |
|-------------|------|
| One seed row per control when no CaC binding | Seed lists multiple controls |
| Id = `gspp_impl_` + control with `.` → `_`, lower | Typo / wrong control |
| After real CaC attach, seed row gone for that control | Dual listing |

### Shared rules across controls

CSV intentionally shares rules (e.g. faillock rules on `BER.3.8 BER.3.10 BER.3.11`).
That is **not** a finding.

Flag **NOTE** only when the same rule appears on controls with no shared theme
(possible accidental `Control_Id_List` append).

### How to extract quickly

```bash
# CSV rows mentioning a control
python3 - <<'PY'
import csv, sys
cid = sys.argv[1]
path = "data/gs-plus-plus-rhel-host-rhel9.csv"
with open(path, newline="", encoding="utf-8") as f:
    rows = list(csv.DictReader(f))
# skip human header row if still present as data
for r in rows:
    ids = (r.get("$$Control_Id_List") or "").split()
    if cid in ids:
        print(r["$$Rule_Id"], ids)
PY

# Markdown Rules bullets
rg -n "^### Rules:" -A20 "md_components/**/{CONTROL}.md"
```

Prefer Python `csv` for quoted fields. Prefer structured JSON access over
eyeballing the whole `component-definition.json`.

## Repetition

### Intra-control

Flag when:

- Two sentences assert the same fact with only synonym swaps.
- The same doc URL or product claim appears twice in one prose block.
- Prose restates the control statement almost verbatim and adds no RHEL
  mechanism.

**WARNING** if ≥2 redundant sentences; else **NOTE**.

### Cross-control copy-paste

Compare prose of siblings (same folder or shared rules). Near-duplicate
full paragraphs are OK when controls share one mechanism **and** each file
still names its distinct obligation (e.g. source authorization vs integrity
check).

**WARNING** when two controls with different statements share nearly identical
prose and neither differentiates (assessor cannot tell them apart).

## Contradiction

### Within prose

Examples:

- “RHEL always verifies GPG” + “signature check is not available”
- “fully covered on host” + long list of uncovered organizational gaps while
  status is `implemented` without saying gaps are out of scope

### Status vs prose

| Status | Conflict if prose… |
|--------|-------------------|
| `implemented` | Lists material uncovered technical requirements with no “org-only” framing |
| `not-applicable` | Describes concrete on-host enforcement as the solution |
| `planned` | Describes current shipped RHEL behavior as already meeting the control |
| `partial` / `alternative` | Rarely CRITICAL; flag only if prose claims full coverage |

Align with enrich status criteria in
[enrich reference](../enrich-component-implementation/reference.md#implementation-status).

### Prose vs rules

If `### Rules:` lists hardening checks for mechanism M, prose must not claim M
does not exist on RHEL (unless status is `alternative` and prose explains the
substitute). If prose centers on M but Rules are empty / only `gspp_impl_*`,
that is Path B — **NOTE** only if prose oversells as if CaC-verified.

## Related controls

### Building the group

1. Same area directory (`…/DET.5/`).
2. Controls sharing any CSV rule row.
3. Explicit user list.

### Compatible vs divergent

Compatible (OK):

- Same subsystem described with consistent facts (Satellite monitors
  repo→host; org owns supplier→repo).
- Different status because statements differ (one org-only, one technical).

Divergent (**WARNING** / **CRITICAL** if assessor-facing):

- Control A: “only Red Hat CDN is trusted.” Control B: “third-party repos are
  equally authorized by default.” (same DET.5.10 source theme)
- Control A: faillock unlock is manual only. Control B: unlock is always
  automatic after N seconds — for the same default story without param context.
- One sibling says feature absent; another says fully implemented — same feature.

Document each finding with short quotes from both sides.

## Wording

Track a small glossary while reviewing. Prefer the dominant sibling form.

| Concept | Prefer (RHEL 9 host) | Avoid unless quoting history |
|---------|----------------------|------------------------------|
| Package manager | DNF | yum as the primary verb for RHEL 9 |
| Product short name | RHEL / Red Hat Enterprise Linux | “Linux”, “the OS” alone when product matters |
| Doc footer | `Weitere Informationen:` | “See also”, bare URL lists without label |
| Auth stacks | `system-auth` / `password-auth` + authselect | Vague “PAM config” only, if siblings name files |

German register: formal, assessor-facing, 2–5 sentences typical. Flag English
paragraph dumps as **NOTE** unless quoting identifier names.

Do not demand identical phrasing — demand **one term per concept** within a
related group.

## Report examples

### Rules mismatch

```markdown
### CRITICAL CSV/MD Rules diverge for DET.5.10.4
- **Controls:** DET.5.10.4
- **Category:** rules
- **Evidence:** CSV has `ensure_gpgcheck_never_disabled`; markdown `### Rules:` omits it
- **Why it matters:** Assemble/CI may disagree with what authors think is mapped
- **Suggested fix:** Re-run csv-to-oscal-cd + regenerate_components.sh; commit CSV+MD+JSON
```

### Related contradiction

```markdown
### WARNING DET.5.10.1 vs DET.5.10.4 disagree on third-party repos
- **Controls:** DET.5.10.1, DET.5.10.4
- **Category:** related
- **Evidence:** DET.5.10.1 allows org-approved extra repos; DET.5.10.4 implies only Red Hat keys ever apply
- **Why it matters:** Assessor may think integrity story forbids approved mirrors
- **Suggested fix:** Align DET.5.10.4 prose with configured keys for freigegebene Repos
```

## Out of scope

- Catalog/profile correctness (wrong statement text) — profile/catalog repos.
- Writing new enrich content (use enrich skill).
- Running full trestle CI unless needed to confirm a CRITICAL sync issue.
- Translating prose language unless user asks.

# Reference — check-component-consistency

## Severity

| Level | Use when |
|-------|----------|
| **CRITICAL** | CSV↔JSON rule map broken; orphan IR; dual CaC+seed; MD advertises wrong/missing **CaC** rule ids; clear factual contradiction that would mislead an assessor |
| **WARNING** | Related controls disagree on a **shared** mechanism; status vs prose mismatch; heavy copy-paste across distinct obligations; invalid status token; prose cites CaC rule ids |
| **NOTE** | Mild intra-control repetition; seed display gap when FM also missing seed; rare accidental shared-rule; small-scope enrich-style nits |

When unsure between CRITICAL and WARNING → **WARNING**.  
When unsure whether to flag at all → **do not flag**.

## Precision (anti false-positive)

Lessons from full-repo audits that produced noise:

| Noise pattern | Correct handling |
|---------------|------------------|
| Missing `### Rules:` on **seed-only** controls while FM lists `gspp_impl_*` and CSV=JSON | **Ignore** (display incomplete; SoT intact) |
| Missing `### Rules:` on seed-only and FM also empty | One clustered **NOTE** in batch — not CRITICAL/WARNING |
| Missing/stale `### Rules:` when CSV/JSON have **CaC** ids | **CRITICAL** (assessor-facing binding wrong) |
| Frontmatter “rules” parsed from params / param-values | **Bug** — only `x-trestle-comp-def-rules.*.name` |
| Separate CRITICAL for FM and MD for same sync defect | Merge into **one** finding per control |
| “Cross-area shared CaC rules” with clear theme (AIDE, GPG, faillock, SELinux) | **Ignore** — intentional CSV reuse |
| Catalog-related pairs with different obligations, status spread, “worth skim” | **Ignore** — not a finding |
| Related pair concluded OK / compatible / justified | **Silence** (Clean section optional) |
| Mass “missing `Weitere Informationen:`” on `all` | **Out of scope** for full-repo; enrich concern |
| Path spelling `system-auth` vs `/etc/pam.d/system-auth` | **Ignore** |
| `/etc/yum.repos.d/` or package history mentioning yum | **Ignore** — not a DNF wording fail |
| `partial`/`alternative` listing org gaps | **Ignore** — expected honesty, not contradiction |

### Pre-emit filter

Delete any drafted finding that:

- Ends with “no change needed”, “OK”, “compatible”, “usually intentional”, or
  “justified”.
- Restates that different control statements have different status.
- Is a second ticket for the same CSV/JSON/MD sync defect.
- Is enrich style (doc footer, tone length) on a full-repo `all` run.

Batch mode: one finding per issue **class** with a control-id list; lead with
CRITICAL SoT issues; keep WARNING/NOTE counts small.

## Rule references

### Triple match (per control)

Build sets for control `C`:

1. **CSV** — every `$$Rule_Id` whose `$$Control_Id_List` contains `C`
2. **JSON** — `Rule_Id` props on the IR for `C`
3. **MD** — bullets under `### Rules:` (absent heading → empty set)
4. **FM** — names under `x-trestle-comp-def-rules` only (ignore params)

| Comparison | Result |
|------------|--------|
| CSV ≠ JSON | **CRITICAL** always |
| CSV/JSON contain CaC id(s); MD missing them or lists extra CaC ids | **CRITICAL** |
| CSV/JSON are seed-only; FM lists seed; MD has no `### Rules:` | **Ignore** |
| CSV/JSON seed-only; FM and MD both omit seed | Clustered **NOTE** |
| FM names ≠ CSV but MD Rules already match CSV | Prefer single MD/CSV finding; FM is regenerate collateral |
| FM parser includes param ids | Invalid — re-parse; do not file |

### Prose must not sell rule ids

Flag **WARNING** if prose contains:

- CaC-style snake_case rule ids (`ensure_gpgcheck_globally_activated`, …)
- `gspp_impl_*` seeds cited as checks or products

Config keys (`gpgcheck`), package names, and paths are fine.

### Narrative seeds

| Expectation | Fail |
|-------------|------|
| One seed row per control when no CaC binding | Seed lists multiple controls → **CRITICAL** |
| Id = `gspp_impl_` + control with `.` → `_`, lower | Typo / wrong control → **CRITICAL** |
| After real CaC attach, seed row gone for that control | Dual listing → **CRITICAL** |

Missing MD `### Rules:` bullet for a seed is **not** a seed-naming failure.

### Shared rules across controls

CSV intentionally shares rules (e.g. faillock on `BER.3.8 BER.3.10 BER.3.11`,
AIDE across DET/KONF/BER, GPG on patch + package integrity). **Default: no
finding.**

**NOTE** only if the same rule lands on controls with no shared mechanism and
no plausible thematic link (suspect accidental `Control_Id_List` append). When
unsure → ignore.

### How to extract quickly

```bash
# CSV rows mentioning a control
python3 - <<'PY'
import csv, sys
cid = sys.argv[1]
path = "data/gs-plus-plus-rhel-host-rhel9.csv"
with open(path, newline="", encoding="utf-8") as f:
    rows = list(csv.DictReader(f))
for r in rows:
    ids = (r.get("$$Control_Id_List") or "").split()
    if cid in ids:
        print(r["$$Rule_Id"], ids)
PY

# Markdown Rules bullets
rg -n "^### Rules:" -A20 "md_components/**/{CONTROL}.md"

# Frontmatter rule names only (not params)
rg -n "x-trestle-comp-def-rules" -A30 "md_components/**/{CONTROL}.md"
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

**WARNING** if ≥2 redundant sentences; else **NOTE**. Skip on tiny prose.

### Cross-control copy-paste

Compare **primary** siblings (same folder or shared rules) before catalog
links. Near-duplicate paragraphs are OK when controls share one mechanism
**and** each file still names its distinct obligation.

**WARNING** only when different statements share nearly identical prose and
neither differentiates (assessor cannot tell them apart). Similarity score
alone is not enough — check obligation differentiation.

## Contradiction

### Within prose

Examples that **do** qualify:

- “RHEL always verifies GPG” + “signature check is not available”
- “fully covered on host” + long list of uncovered **technical** gaps while
  status is `implemented` without org-only framing

Do **not** flag honest `partial`/`alternative` scope limits.

### Status vs prose

| Status | Conflict if prose… |
|--------|-------------------|
| `implemented` | Lists material uncovered **technical** requirements with no “org-only” framing |
| `not-applicable` | Describes concrete on-host enforcement as the solution |
| `planned` | Describes current shipped RHEL behavior as already meeting the control |
| `partial` / `alternative` | Flag only if prose claims full coverage |
| unknown token (e.g. `operational`) | **WARNING** (wording / status vocabulary) |

Align with enrich status criteria in
[enrich reference](../enrich-component-implementation/reference.md#implementation-status).

### Prose vs rules

If MD/`### Rules:` lists **CaC** hardening checks for mechanism M, prose must
not claim M does not exist on RHEL (unless `alternative` + substitute explained).
Path B seed-only → never treat “no automated CaC check” as contradiction.

## Related controls

### Building the group

1. Same area directory (`…/DET.5/`) — primary.
2. Controls sharing any CSV rule row — primary.
3. Explicit user list — primary.
4. Catalog `related` / links — **secondary**; include only when a shared
   mechanism/theme is obvious. Do not fan out the whole link graph on `all`.

### Compatible vs divergent

Compatible (**no finding**):

- Same subsystem, consistent facts (Satellite monitors repo→host; org owns
  supplier→repo).
- Different status because statements differ (org-only vs technical).
- Shared rule, differentiated prose (e.g. UID recycle vs delete/retention).

Divergent (**WARNING**; **CRITICAL** only if assessor would be misled):

- Control A: “only Red Hat CDN is trusted.” Control B: “third-party repos are
  equally authorized by default.” (same source theme, incompatible facts)
- Control A: faillock unlock manual only. Control B: unlock always automatic —
  same default story, no param context.
- One sibling: feature absent; another: fully implemented — same feature.

Always quote both sides. Never file “status spread across different
obligations” or “optional human skim” as findings.

## Wording

Track a small glossary while reviewing. Prefer the dominant sibling form.

| Concept | Prefer (RHEL 9 host) | Avoid unless quoting history |
|---------|----------------------|------------------------------|
| Package manager | DNF as install/update verb | “yum install” as primary RHEL 9 guidance |
| Repo paths | `/etc/yum.repos.d/…` OK | Treating repo path as wording defect |
| Product short name | RHEL / Red Hat Enterprise Linux | “Linux”, “the OS” alone when product matters |
| Auth stacks | `system-auth` / `password-auth` + authselect | Demanding full path every time |
| Status tokens | Allowed set only | `operational`, `complete`, … |

German register: formal, assessor-facing. English identifier quotes OK.

**Doc footers (`Weitere Informationen:`):** enrich convention, not SoT. Skip on
full-repo `all` runs. For a single control / small group, at most one **NOTE**
if in-scope siblings all have footers and the target does not.

Do not demand identical phrasing — demand **one term per concept** within a
primary related group when drift would confuse an assessor.

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

### Do not report (false-positive examples)

- Seed-only control: FM has `gspp_impl_det_5_10_1`, CSV=JSON, no `### Rules:` heading.
- “AIDE shared across BER.7.11 + DET.4.* + KONF.7.* — usually intentional.”
- “DET.4.2 ↔ KONF.7.* status spread; different obligations — worth skim.”
- “BER.3.14 ↔ BER.3.15 differentiated — OK.”
- Twenty-nine controls missing `Weitere Informationen:` on an `all` run.

## Out of scope

- Catalog/profile correctness (wrong statement text) — profile/catalog repos.
- Writing new enrich content (use enrich skill).
- Running full trestle CI unless needed to confirm a CRITICAL sync issue.
- Translating prose language unless user asks.
- Full-repo doc-footer completeness sweeps (`Weitere Informationen:`).
- “Compatible related group” write-ups with no defect.
- Seed-only missing `### Rules:` when frontmatter already lists the seed.

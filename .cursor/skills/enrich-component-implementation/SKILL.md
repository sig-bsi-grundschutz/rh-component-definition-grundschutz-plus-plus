---
name: enrich-component-implementation
description: >-
  Enrich GS++ trestle component markdown with German implementation prose from
  ComplianceAsCode rules and Red Hat documentation, attach rules via the CSV
  rule map (or a narrative gspp_impl_* seed when no CaC rule applies), sync
  Rules/parameters into JSON, and open a PR with markdown + CSV + JSON. Use when
  authoring or updating implementation answers under md_components/, mapping CaC
  rules into data/*.csv, enriching controls without CaC coverage, or documenting
  how a GS++ control is implemented with a specific RH product.
---

# Enrich Component Implementation

Turn one or more trestle **component markdown** files into reviewed PRs that
include:

1. German implementation prose + status in markdown
2. CSV mapping: real CaC rules **or** a narrative `gspp_impl_*` seed when no
   attachable CaC rule exists (see [Non-CaC controls](#non-cac-controls--narrative-seeds))
3. Synced OSCAL JSON with `Rule_Id` (CaC or seed), optional `set-parameters`,
   description, and status

This repo is the **component-definition** trestle template instance
(`config.env` → `COMPONENT_DEFINITION`). Profile scope lives in the sibling
`rh-profile-grundschutz-plus-plus` repo; catalog in `rh-catalog-grundschutz-plus-plus`
(vendored here under `catalogs/grundschutz-plus-plus/`).

**Rule source of truth:** `data/gs-plus-plus-rhel-host-rhel9.csv` (path from
`data/csv-to-oscal-cd.config`). Do **not** hand-edit `Rule_Id` /
`set-parameters` in JSON, and do **not** edit `### Rules:` in markdown
(display-only; filled by regenerate from JSON).

**Why CSV always needs a row:** `csv-to-oscal-cd` + assemble only keep an
implemented-requirement when the control appears in a CSV `$$Control_Id_List`.
Controls with markdown but no CSV row become orphans and assemble drops their
prose. For non-CaC controls use a narrative seed row, not a fake CaC id.

**Input:** one of:
- path under `md_components/{COMPONENT_DEFINITION}/…/{control-id}.md`, or
- one or more **control IDs** (e.g. `BER.2.4` or `KONF.2.1, DET.3.1.4`).
  Default artifact = `COMPONENT_DEFINITION` from `config.env`
  (`gs-plus-plus-rhel-host-rhel9`).

**Output:** one branch + commit + PR **per control** (see Git safety). Each PR
must contain, for that control:

| Artifact | Role |
|----------|------|
| `data/{COMPONENT_DEFINITION}.csv` | CaC rule map **or** `gspp_impl_*` seed (+ params if any) |
| `md_components/…/{control-id}.md` | Prose + status (`### Rules:` from sync) |
| `component-definitions/{COMPONENT_DEFINITION}/component-definition.json` | `Rule_Id`, params, description, status |

Merge target is **`develop`**. CI (`dev-push.yml` → `check_and_update_all.sh`)
re-runs CSV→JSON and assemble; committing CSV + MD + JSON together keeps prose
and rules aligned.

## Prerequisites

| Resource | Default path | Override |
|----------|--------------|----------|
| This repo | workspace root | — |
| `COMPONENT_DEFINITION` | `config.env` | — |
| Rule CSV | `data/{COMPONENT_DEFINITION}.csv` | `data/csv-to-oscal-cd.config` |
| CSV→OSCAL config | `data/csv-to-oscal-cd.config` | — |
| CaC-content clone | `../CaC-content` | env `CAC_CONTENT_ROOT` |
| Vendored catalog | `catalogs/grundschutz-plus-plus/catalog.json` | — |
| Profile | `profiles/gs-plusplus-*` | — |
| RHEL major | `9` (from component title *Red Hat Enterprise Linux 9*) | — |
| Trestle | repo `.venv` (`pip install -r requirements.txt`) | — |

Requires `git`, `gh`, network for push/PR. Activate trestle before sync steps:

```bash
source .venv/bin/activate   # or: python3 -m pip install -r requirements.txt
```

The resources named in Step 2 — Red Hat documentation — shall be available.
Check and verify before enriching. If they are not available, stop and ask the
user how to proceed.

## Git safety

1. Run `git branch --show-current`. **Never commit on that branch.**
2. Branch from current **`develop`** (or `origin/develop`): `cursor/implement-{control-id}`.
3. One control per PR unless the user asks for a batch. For a set, repeat Steps 0–9
   per control, each branch from the **same original `develop` HEAD** (not stacked).

## Workflow

```
- [ ] 0. Resolve target markdown (stop if control not in this repo yet)
- [ ] 1. Read and parse component markdown
- [ ] 2. Gather Red Hat documentation
- [ ] 3. Discover CaC rules (find-rule) + load rule.yml / *.var
- [ ] 3b. If no attachable CaC rule → choose narrative-seed path (gspp_impl_*)
- [ ] 4. Update CSV (CaC rows and/or gspp_impl_* seed) for this control
- [ ] 5. Sync CSV → JSON → markdown Rules (csv-to-oscal-cd + regenerate)
- [ ] 6. Draft German implementation prose
- [ ] 7. Evaluate coverage → set Implementation Status
- [ ] 8. Write markdown (preserve protected sections)
- [ ] 9. Assemble markdown → JSON; branch, commit CSV+MD+JSON, open PR → develop
```

## Non-CaC controls — narrative seeds

Some profile-selected controls have **no** strong/partial ComplianceAsCode rule
with `cce@rhel{N}`. Still enrich them: docs + honest prose + status. Do **not**
invent CaC rule ids or force weak matches into the CSV.

**Constraint:** without a CSV row that lists the control, assemble will not keep
an IR for it. Use a **narrative seed** row instead of a CaC rule:

| Field | Value |
|-------|--------|
| `$$Rule_Id` | `gspp_impl_{control_id}` with dots → `_`, lowercased (e.g. `BER.3.14` → `gspp_impl_ber_3_14`) |
| `$$Rule_Description` | `Narrative implementation seed for {CONTROL_ID} (no CaC rule binding)` |
| `$$Control_Id_List` | exactly this control id (one seed row per control; do not share) |
| Parameters | leave empty |
| Other columns | same constants as sibling data rows (`$$Component_Title`, profile, namespace, …) |

**After Step 3:**

1. **Attachable CaC rules exist** → Path A: Step 4 adds/updates those CaC rows
   (preferred). If a `gspp_impl_*` seed already exists for the control and you
   attach a real CaC rule, **remove this control id from the seed row** (delete
   the seed row if its list becomes empty) so `### Rules:` is not dual-listed.
2. **No attachable CaC rules** → Path B: ensure a `gspp_impl_*` seed row exists
   (create if missing). Continue Steps 5–9. `### Rules:` showing `gspp_impl_*`
   is expected; do not cite that id in prose as if it were a CaC check.
3. Weak/speculative CaC matches → PR "Considered but not attached" only.

Status when Path B: cannot be `implemented`. Use `alternative`, `partial`,
`planned`, or `not-applicable` per [reference.md](reference.md#implementation-status).

### Step 0 — Resolve target

Search:

```text
md_components/{COMPONENT_DEFINITION}/**/{control-id}.md
```

Typical path:

```text
md_components/gs-plus-plus-rhel-host-rhel9/Red Hat Enterprise Linux 9/gs-plusplus-rhel-host/{area}/{control-id}.md
```

**If found:** continue to Step 1.

**If not found:** do **not** hand-create markdown or edit `profiles/` here.
New controls enter via the **profile** repo (`md_profiles` → assemble →
`profiles_autoupdate` PR into this repo’s `develop` →
`regenerate_components.sh`). Tell the user the control is missing and that it
must be included upstream first. Stop.

### Step 1 — Read and parse

Read the target markdown. Extract:

- **Control ID** — from `#` heading (e.g. `BER.2.4`)
- **Control Statement** — `## Control Statement` (read-only)
- **Control guidance** — `## Control guidance` (read-only)
- **Current prose** — between HTML comments and `### Rules:` (or `### Implementation Status:` if no Rules heading)
- **Current status** — `### Implementation Status: {value}`
- **Current Rules** — `### Rules:` bullets (display from JSON; may already list rules)

Also note existing CSV rows that already list this control in `$$Control_Id_List`
(Step 4 merges; do not duplicate).

Preserve YAML frontmatter unchanged except what regenerate rewrites for
`x-trestle-comp-def-rules` / `x-trestle-rules-params` / param vals. Leave empty
`x-trestle-param-values` catalog placeholders alone — those are **not** CaC
rule vars.

### Step 2 — Red Hat documentation

Target product/version from the component (e.g. **Red Hat Enterprise Linux 9**).
Every doc hit must match that product (and major version). Discard wrong-product
hits (other RHEL majors, OpenShift-only, Ansible-only, etc. unless the control
clearly needs them).

**1. Primary — MCP** `user-Red-Hat-documentation` (or `redhat-documentation-mcp`).
Probe with `GetDynamicTools` first. Query for the control topic + product.
If unavailable or slow: **one** retry max, then continue to RHOKP.

**2. Secondary — `rhokp-docs`** if `../rhokp-scraper` is in the workspace — read
`../rhokp-scraper/.cursor/skills/rhokp-docs/SKILL.md`. Prefer already-scraped
markdown under
`../rhokp-scraper/output/red_hat_enterprise_linux/{rhel_major}/` (`rg` first).
Only scrape/search if missing, and only if RHOKP is up
(`curl -fsS "$RHOKP_BASE_URL"`).

**RHOKP failure = hard stop.** Treat as failure when: scraper repo missing,
portal unreachable, scrape/search errors, or no usable product-matching content
after trying. Then **stop the workflow**, tell the user MCP + RHOKP did not
yield usable docs, and **ask for explicit permission** to use web search.
Do **not** call WebSearch, WebFetch, or any other public-web doc fetch until
the user clearly grants that permission.

**3. Web search (only after explicit user permission):**
- Use WebSearch restricted to **`site:docs.redhat.com`** (any page on that
  host — not limited to a fixed topic list).
- Double-check each result: product name + version match the component
  (e.g. path/title contains `red_hat_enterprise_linux/9` for this artifact).
  Reject wrong product/version.
- Optionally WebFetch accepted URLs to pull detail for prose.

If the user denies web search: continue with whatever MCP/RHOKP already gave
(and later CaC rule text from Step 3 for mechanism research only). Never
fabricate doc URLs. Record doc source used for the PR body.

### Step 3 — Discover CaC rules

1. Read `{CAC_CONTENT_ROOT}/.claude/skills/find-rule/SKILL.md`.
2. Run it with control statement + guidance; scope `linux_os/guide/` (skip OpenShift).
3. Keep strong/partial matches with `cce@rhel{N}`.
4. Load matching `rule.yml` files (`title`, `description`, `rationale`, template
   vars, `ocil` / `fixtext`).
5. For each rule variable referenced (e.g. `xccdf_value("var_…")`), load the
   sibling `var_….var` file: `options` keys/values and `default` → CSV parameter
   columns (see [reference.md](reference.md#csv-rule-map)).

**Path A — attachable matches:** attach in Step 4 (CSV), not as PR-only notes.

**Path B — none attachable:** do not invent CaC ids. Go to narrative seed
([Non-CaC controls](#non-cac-controls--narrative-seeds)). Still list weak /
speculative matches under PR "Considered but not attached".

### Step 4 — Update CSV

Edit `data/{COMPONENT_DEFINITION}.csv` (UTF-8, preserve the two header rows).

**Row model:** one row per `$$Rule_Id`. `$$Control_Id_List` is a
**space-separated** list of control IDs sharing that rule.

#### Path A — CaC rules

For each strong/partial rule from Step 3:

1. **Rule already in CSV:** add this control ID to `$$Control_Id_List` if missing.
   Update parameter columns only when this enrichment chooses a value and the
   row has none (or the user asked to change the default).
2. **New rule:** append a row. Copy constants from any existing data row:
   - `$$Component_Title` = `Red Hat Enterprise Linux 9`
   - `$$Component_Description` = same as sibling data rows (currently
     `Red Hat Enterprise Linux 9`)
   - `$$Component_Type` = `software`
   - `$$Profile_Source` = `trestle://profiles/gs-plusplus-rhel-host/profile.json`
   - `$$Profile_Description` = same as sibling rows (must match
     `control-implementation.description` for merge)
   - `$$Namespace` = `https://oscal-compass.github.io/compliance-trestle/schemas/oscal/cd`
   - `$$Rule_Id` = CaC rule directory name
   - `$$Rule_Description` = `description` element from the rule-file in ComplianceAsCode Repository
   - `$$Control_Id_List` = this control id (and any others if intentional)
   - Parameter columns from `.var` when the rule has variables — see
     [reference.md](reference.md#csv-rule-map). Use `_1` suffix columns for a
     second parameter on the same rule.

If replacing a narrative seed with real CaC rules, drop this control from the
`gspp_impl_*` seed row (delete the seed row when empty).

#### Path B — narrative seed (no CaC rule)

1. If `gspp_impl_{control}` already lists this control → leave CSV as-is for
   mapping (still proceed to sync if other edits need it).
2. Else append one seed row per
   [Non-CaC controls](#non-cac-controls--narrative-seeds).

**Do not** remove other controls from an existing CaC row’s `$$Control_Id_List`.

Validate with Python `csv` module (quoted fields, commas inside descriptions).
Do not break the two header rows (`$$…` then human labels). Prefer `csv`
module edits over hand-rewriting the whole file (preserves UTF-8 umlauts).

### Step 5 — Sync CSV → JSON → markdown Rules

Ensure `data/csv-to-oscal-cd.config` sets `component-definition =` to the
existing JSON path so trestle **merges** and preserves UUIDs. Without that key,
trestle recreates the CD and churns all UUIDs.

`trestle task csv-to-oscal-cd` **merges** into the existing
`component-definition.json` (adds/mods/deletes rules and control mappings;
preserves existing IR `description` / status). Then regenerate refreshes
markdown `### Rules:` and rule frontmatter from JSON.

```bash
source .venv/bin/activate # or: python3 -m pip install -r requirements.txt
trestle task csv-to-oscal-cd -c data/csv-to-oscal-cd.config
./scripts/automation/regenerate_components.sh
```

Confirm for this control:

- JSON IR exists and has `Rule_Id` props (CaC ids and/or `gspp_impl_*`)
- CI-level `set-parameters` when CaC defaults set
- Markdown `### Rules:` lists those rule ids (seed id is OK on Path B)
- Frontmatter rule/param blocks updated if parameters exist

If `validate-controls = on` fails (control not in profile): stop and report —
do not force the mapping.

### Step 6 — Draft implementation prose

German prose only between the HTML comments and `### Rules:` /
`### Implementation Status:`.

- How RHEL implements the control on-host (PAM, sshd, LUKS, auditd, SSSD, …)
- **No** CaC/OpenSCAP/rule IDs in prose — research only (Steps 2–3), cite in PR /
  matrix. See [reference.md](reference.md#implementation-prose).
- Honest limits (org/IAM/process gaps)
- 2–5 sentences; match tone of sibling files under `md_components/`
- End with **"Weitere Informationen:"** + public doc URL(s) from Step 2 —
  [reference.md](reference.md#linking-the-source-of-truth). Skip only if no doc
  source; never invent URLs.

Leave both HTML comments intact.

### Step 7 — Implementation status

Set `### Implementation Status:` per [status criteria](reference.md#implementation-status).

| Status | When |
|--------|------|
| `implemented` | Strong Step 3 **CaC** rules (now in CSV/JSON) + docs fully cover technical statement **and** guidance. Not available on Path B (narrative seed only). |
| `partial` | Rules/docs cover some aspects; gaps remain. Always true if the control has organizational aspects |
| `alternative` | Different technical approach, same intent (common on Path B when host offers lock/preserve/IdM instead of a literal check) |
| `planned` | No CaC rule, no narrative mechanism in docs, and no defensible on-host approach yet |
| `not-applicable` | Organizational-only; no RHEL host hook |

Unsure `implemented` vs `partial` → **`partial`**. Judge the end state after
Steps 4–5 (CaC rules or seed attached), not the pre-enrichment empty Rules list.
A `gspp_impl_*` seed alone never justifies `implemented`.

Coverage matrix for PR body — [reference.md](reference.md#coverage-matrix).

### Step 8 — Write markdown

Edit **only**:

- Prose between comments and Rules/Status
- `### Implementation Status: {value}`

**Do not change:** title, Control Statement/guidance, `### Rules:` (already from
Step 5), HTML comments, separators. Prefer not hand-editing rule frontmatter —
regenerate owns it.

### Step 9 — Assemble, branch, commit, PR

Assemble so JSON picks up prose + status **after** Rules/params are already in
JSON from Step 5:

```bash
source .venv/bin/activate # or: python3 -m pip install -r requirements.txt
./scripts/automation/assemble_components.sh
# optional: python3 -m trestle validate -a
```

Verify JSON IR for this `control-id`: non-empty `description`,
`implementation-status` prop, `Rule_Id` props (CaC and/or `gspp_impl_*`), and
expected `set-parameters` when Path A set defaults. After assemble, confirm
root/component UUIDs unchanged vs pre-sync (merge config working).

Then:

```bash
git fetch origin
git checkout -b cursor/implement-{control-id} origin/develop
git add \
  "data/{COMPONENT_DEFINITION}.csv" \
  "md_components/.../{control-id}.md" \
  "component-definitions/{COMPONENT_DEFINITION}/component-definition.json"
git status --short
git commit -m "$(cat <<'EOF'
Enrich {control-id}: prose, CSV rules, and component JSON.

Assisted-by: Cursor
EOF
)"
git push -u origin HEAD
gh pr create --base develop --title "Enrich {control-id} component implementation" --body "$(cat <<'EOF'
## Summary
- Updated German implementation prose for `{control-id}`
- Implementation status: `{status}`
- Path A: attached CaC rules via CSV → JSON
  **or** Path B: narrative seed `gspp_impl_*` (no CaC rule binding)

## Coverage matrix
| Guidance aspect | Covered by | Status |
|-----------------|------------|--------|
| ... | ... | ... |

## Documentation sources
- ...

## CaC rules (CSV → JSON)
- `rule_id` — rationale
- parameters (if any): `param-id` = `value` (alternatives: …)
- (Path B) none — seed `gspp_impl_…` only

## Narrative seed (Path B only)
- `gspp_impl_…` — why no CaC rule; what on-host/org approach prose describes

## Considered but not attached
- …

## Gaps / manual verification
- ...

## Test plan
- [ ] Review prose (no inline rule IDs; seed id not sold as CaC check)
- [ ] Confirm CSV `Control_Id_List` / parameters or `gspp_impl_*` seed for this control
- [ ] Confirm markdown `### Rules:` matches CSV (CaC and/or seed)
- [ ] Confirm JSON IR: description, implementation-status, Rule_Id, set-parameters
- [ ] Confirm component-definition UUIDs preserved (merge config present)
- [ ] After merge to develop: confirm CI Autoupdate did not regress prose/rules
EOF
)"
```

**Commit CSV + MD + JSON** whenever the CSV changed (new CaC row, seed row, or
`Control_Id_List` edit). Always commit MD + JSON after assemble. Skipping JSON
risks CI `csv-to-oscal-cd` + regenerate running before assemble has the new
prose in JSON, which can wipe uncommitted description text from markdown.

Do **not** rewrite JSON with `json.dumps(..., ensure_ascii=True)` — that turns
German umlauts into `\u00xx` escapes. Prefer trestle assemble/write, or
`ensure_ascii=False` if a manual JSON edit is unavoidable.

Return the PR URL.

## Example invocation

```
Enrich md_components/gs-plus-plus-rhel-host-rhel9/Red Hat Enterprise Linux 9/gs-plusplus-rhel-host/BER.2/BER.2.4.md
```

```
Enrich BER.2.4
```

```
Enrich BER.3.14
```
(Path B when find-rule has no attachable CaC match: add `gspp_impl_ber_3_14` seed.)

```
Enrich KONF.2.1, KONF.2.2 and DET.3.1.4
```

## Additional resources

- [Status criteria, CSV columns, docs hints](reference.md)
- `config.env` — `COMPONENT_DEFINITION`
- `data/csv-to-oscal-cd.config` — CSV path and output dir
- `scripts/automation/assemble_components.sh` / `regenerate_components.sh` / `check_and_update_all.sh`
- Sibling profile repo: `../rh-profile-grundschutz-plus-plus` (include new controls there first)

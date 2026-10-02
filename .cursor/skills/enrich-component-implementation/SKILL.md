---
name: enrich-component-implementation
description: >-
  Enrich GS++ trestle component markdown in this repo with German implementation
  prose from ComplianceAsCode rules and Red Hat documentation, evaluate coverage,
  set implementation status, and open a PR on a separate branch. Use when
  authoring or updating implementation answers under md_components/, or when the
  user asks to document how a GS++ control is implemented with a specific RH Product.
---

# Enrich Component Implementation

Turn one or more trestle **component markdown** files into reviewed PRs with honest
implementation prose, coverage assessment, and optional CaC rule suggestions.

This repo is the **component-definition** trestle template instance
(`config.env` → `COMPONENT_DEFINITION`). Profile scope lives in the sibling
`rh-profile-grundschutz-plus-plus` repo; catalog in `rh-catalog-grundschutz-plus-plus`
(vendored here under `catalogs/grundschutz-plus-plus/`).

**Input:** one of:
- path under `md_components/{COMPONENT_DEFINITION}/…/{control-id}.md`, or
- one or more **control IDs** (e.g. `BER.2.4` or `KONF.2.1, DET.3.1.4`).
  Default artifact = `COMPONENT_DEFINITION` from `config.env`
  (`gs-plus-plus-rhel-host-rhel9`).

**Output:** one branch + commit + PR **per control** (see Git safety). Default
skill commit is **markdown only** (prose + status). Push/merge target is
**`develop`** — CI (`dev-push.yml` → `check_and_update_all.sh`) runs
`trestle author component-assemble` and commits JSON. Attaching `Rule_Id` /
`set-parameters` is a **JSON** edit (see Step 7 / reference); never via
`### Rules:` in markdown.

## Prerequisites

| Resource | Default path | Override |
|----------|--------------|----------|
| This repo | workspace root | — |
| `COMPONENT_DEFINITION` | `config.env` | — |
| CaC-content clone | `../CaC-content` | env `CAC_CONTENT_ROOT` |
| Vendored catalog | `catalogs/grundschutz-plus-plus/catalog.json` | — |
| Profile (source of applicable controls) | `profiles/gs-plusplus-*` | — |
| RHEL major | `9` (from component title *Red Hat Enterprise Linux 9*) | — |

Requires `git`, `gh`, network for push/PR.

## Git safety

1. Run `git branch --show-current`. **Never commit on that branch.**
2. Branch from current **`develop`** (or `origin/develop`): `cursor/implement-{control-id}`.
3. One control per PR unless the user asks for a batch. For a set, repeat Steps 0–7
   per control, each branch from the **same original `develop` HEAD** (not stacked).

## Workflow

```
- [ ] 0. Resolve target markdown (stop if control not in this repo yet)
- [ ] 1. Read and parse component markdown
- [ ] 2. Gather Red Hat documentation
- [ ] 3. Discover CaC rules (suggestions only, find-rule skill)
- [ ] 4. Draft German implementation prose
- [ ] 5. Evaluate coverage → set Implementation Status
- [ ] 6. Write markdown (preserve protected sections)
- [ ] 7. Branch, commit, push, open PR → develop
```

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

At skill invoke time `### Rules:` and JSON `Rule_Id` are **typically empty** — do
**not** spend a step loading listed rules. CaC research is Step 3 (`find-rule`)
only. Still note any rare pre-existing `### Rules:` / JSON `Rule_Id` /
`set-parameters` for the PR body if present; do not edit `### Rules:`.

Preserve YAML frontmatter unchanged (`x-trestle-global`; leave any empty
`x-trestle-param-values` catalog placeholders alone — those are **not** CaC
rule vars).

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

### Step 3 — Discover CaC rules (PR suggestions only)

1. Read `{CAC_CONTENT_ROOT}/.claude/skills/find-rule/SKILL.md`.
2. Run it with control statement + guidance; scope `linux_os/guide/` (skip OpenShift).
3. Keep strong/partial matches with `cce@rhel{N}`.
4. Load matching `rule.yml` files for research (`title`, `description`,
   `rationale`, template vars, `ocil` / `fixtext`).

**Do not edit `### Rules:` in markdown** — assemble ignores it.
List discoveries in the PR under "Suggested CaC rules".

For rule **variables**, list them in the PR and show the JSON shape to attach
(not markdown frontmatter):

```json
{
  "name": "Rule_Id",
  "ns": "https://oscal-compass.github.io/compliance-trestle/schemas/oscal/cd",
  "value": "selinux_state"
}
```

```json
"set-parameters": [
  {
    "param-id": "var_selinux_state",
    "values": ["enforcing"]
  }
]
```

Optional companion props on the same IR: `Parameter_Id`,
`Parameter_Value_Alternatives` (see existing IRs e.g. `BER.2.5`, `KONF.6.1`).

### Step 4 — Draft implementation prose

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

### Step 5 — Implementation status

Set `### Implementation Status:` per [status criteria](reference.md#implementation-status).

| Status | When |
|--------|------|
| `implemented` | Strong Step 3 CaC rules + docs fully cover technical statement **and** guidance |
| `partial` | Rules/docs cover some aspects; gaps remain. Always true if the control has organizational aspects |
| `alternative` | Different technical approach, same intent — not “no rule attached yet” |
| `planned` | No rule and no doc-backed mechanism |
| `not-applicable` | Organizational-only; no RHEL host hook |

Unsure `implemented` vs `partial` → **`partial`**. Judge anticipated end state
(as if strong Step 3 rules will be attached in JSON), not empty `### Rules:`.

Coverage matrix for PR body — [reference.md](reference.md#coverage-matrix).

### Step 6 — Write markdown

Edit **only**:

- Prose between comments and Rules/Status
- `### Implementation Status: {value}`

**Do not change:** frontmatter, title, Control Statement/guidance, `### Rules:`,
HTML comments, separators.

### Step 7 — Branch, commit, PR

**Default (prose + status only):**

```bash
git fetch origin
git checkout -b cursor/implement-{control-id} origin/develop
git add "md_components/.../{control-id}.md"
# markdown only — do not stage JSON unless attaching rules (below)
git status --short
git commit -m "$(cat <<'EOF'
Enrich {control-id} implementation prose and status.

Assisted-by: Cursor
EOF
)"
git push -u origin HEAD
gh pr create --base develop --title "Enrich {control-id} component implementation" --body "$(cat <<'EOF'
## Summary
- Updated German implementation prose for `{control-id}`
- Implementation status: `{status}`

## Coverage matrix
| Guidance aspect | Covered by | Status |
|-----------------|------------|--------|
| ... | ... | ... |

## Documentation sources
- ...

## Suggested CaC rules
(not attached — edit JSON `Rule_Id` / `set-parameters`; `### Rules:` is display-only)
- `rule_id` — rationale
- suggested `set-parameters` (if any): `param-id` = `value`

## Attaching a rule (reviewer, optional)
1. Prefer merge this markdown PR to `develop` first so CI assembles description/status into JSON.
2. On a follow-up (or same PR only if you already ran assemble locally — see note): edit
   `component-definitions/gs-plus-plus-rhel-host-rhel9/component-definition.json`
   for this `control-id`: add `Rule_Id` props and optional `set-parameters` /
   `Parameter_Id` / `Parameter_Value_Alternatives`.
3. **Do not** edit `### Rules:` expecting assemble to pick it up.
4. **CI hazard:** if the same push changes **both** JSON and markdown, develop CI runs
   `regenerate_components.sh` *before* assemble and can wipe unsynced markdown prose.
   Same-PR rule attach: run `./scripts/automation/assemble_components.sh` locally first
   (prose → JSON), then add `Rule_Id`/`set-parameters` to JSON, commit **both**.

## Gaps / manual verification
- ...

## Test plan
- [ ] Review prose
- [ ] Confirm implementation status matches coverage
- [ ] After merge to develop: confirm CI Autoupdate assembled JSON (or locally:
      `./scripts/automation/assemble_components.sh` then `python3 -m trestle validate -a`)
EOF
)"
```

Return the PR URL.

## Example invocation

```
Enrich md_components/gs-plus-plus-rhel-host-rhel9/Red Hat Enterprise Linux 9/gs-plusplus-rhel-host/BER.2/BER.2.4.md
```

```
Enrich BER.2.4
```

```
Enrich KONF.2.1, KONF.2.2 and DET.3.1.4
```

## Additional resources

- [Status criteria, Rule_Id, docs hints](reference.md)
- `config.env` — `COMPONENT_DEFINITION`
- `scripts/automation/assemble_components.sh` / `regenerate_components.sh` / `check_and_update_all.sh`
- Sibling profile repo: `../rh-profile-grundschutz-plus-plus` (include new controls there first)

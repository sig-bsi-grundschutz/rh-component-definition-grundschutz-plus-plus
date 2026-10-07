# Reference — enrich-component-implementation

## Pipeline (CSV → JSON → markdown → assemble)

Source of truth by concern:

| What | Where | How it lands in the PR |
|------|--------|-------------------------|
| Rule ↔ control map + param defaults | `data/{COMPONENT_DEFINITION}.csv` | Edit in Step 4 |
| `Rule_Id`, component rule props, `set-parameters` | `component-definitions/…/component-definition.json` | `trestle task csv-to-oscal-cd` (merge) |
| `### Rules:` + rule frontmatter | `md_components/…/{control-id}.md` | `regenerate_components.sh` |
| Prose + `implementation-status` | markdown → JSON IR | Edit MD, then `assemble_components.sh` |

`csv-to-oscal-cd` loads the **existing** component-definition and merges rule
add/mod/del and control mappings. Existing IR `description` values are kept;
only **new** controls get an empty description until assemble.

**Develop CI** (`check_and_update_all.sh`) order when paths change:

1. CSV changed → `csv-to-oscal-cd` + regenerate
2. JSON changed → regenerate
3. Markdown changed → assemble

Commit **CSV + MD + JSON** together so CI merge starts from JSON that already
has prose and rules. Markdown-only or CSV-only commits can leave Rules or prose
out of sync until a follow-up.

`### Rules:` is **read-only display** (`trestle.common.const.RULES_WARNING`).
Assemble does **not** write markdown rule bullets into OSCAL.

## CSV rule map

File: `data/gs-plus-plus-rhel-host-rhel9.csv` (see `data/csv-to-oscal-cd.config`).

- Row 1: machine headers (`$$Rule_Id`, `$$Control_Id_List`, `$Parameter_Id`, …)
- Row 2: human descriptions (do not remove)
- Row 3+: one row per rule

Important columns:

| Column | Notes |
|--------|--------|
| `$$Rule_Id` | CaC rule id (directory name under `linux_os/guide/`) |
| `$$Rule_Description` | `description` field from the `rule.yml` in ComplianceAsCode Repository |
| `$$Control_Id_List` | Space-separated control ids (e.g. `BER.2.5 BER.3.9`) |
| `$Parameter_Id` | CaC var id (`var_…`) |
| `$Parameter_Description` | Short label (often var id with `_` → spaces) |
| `$Parameter_Value_Alternatives` | Comma-separated option **values** from `.var` `options` |
| `$Parameter_Value_Default` | Chosen default → CI `set-parameters` |
| `$Parameter_Id_1` … `_Default_1` | Second parameter on the same rule |

Constants copied from sibling rows for this artifact:

- Title: `Red Hat Enterprise Linux 9`
- Type: `software`
- Profile: `trestle://profiles/gs-plusplus-rhel-host/profile.json`
- Namespace: `https://oscal-compass.github.io/compliance-trestle/schemas/oscal/cd`
- `$$Profile_Description` must match existing
  `control-implementation.description` (merge key)

### Parameters from CaC `.var` files

```text
{CAC_CONTENT_ROOT}/linux_os/guide/**/var_{name}.var
```

Example `options` → CSV:

```yaml
options:
    "0": "0"
    30: 30
    35: 35
    default: 35
```

→ `$Parameter_Value_Alternatives` = `0,180,30,35,40,45,60,90` (all option
values, comma-separated; follow existing row style)
→ `$Parameter_Value_Default` = `35`

Empty parameter columns when the rule has no variables.

## Implementation status

Evaluate against the **end state after CSV sync** — rules from Step 3 that were
written into the CSV and merged into JSON. Apply in order; first match wins
unless a higher bar clearly fits.

### `implemented`

All must be true:

1. Every **technical** clause in Control Statement is addressed by strong
   attached CaC rules for the target RHEL major.
2. Every **technical** clause in Control guidance is addressed (as far as
   OS-level controls allow).
3. Those rules have `cce@rhel{N}`.
4. Covering rule IDs appear in CSV / JSON / markdown `### Rules:`.
5. Prose describes the on-host mechanism — not the requirement, not rule IDs
   ([Implementation prose](#implementation-prose)).

### `partial`

Attached rules / docs cover only **part** of statement/guidance:

- Part of guidance only (e.g. `/etc/passwd` watch, not full IdM lifecycle)
- Audit lacks a field guidance asks for
- Product covers mechanism; institution covers process
- Control has organizational aspects (always at least `partial`)

Default when uncertain.

### `alternative`

Genuinely **different** technical approach than literal statement/guidance
(same security intent).

### `planned`

No attached rule **and** no doc-backed mechanism.

### `not-applicable`

Guidance is organizational-only; no defensible RHEL host hook. Do not use to
skip hard partial cases.

## Implementation prose

Prose under **What is the solution and how is it implemented?** = how RHEL
implements the control. CaC/OpenSCAP belong in CSV/JSON `Rule_Id`, PR body, and
coverage matrix — not here.

**DO:**

> OpenSSH authentifiziert Fernwartung über PAM angebunden an SSSD/IdM.

**DON'T:**

> Die Regel `sshd_enable_pam` stellt sicher, dass SSH PAM nutzt.

No inline rule IDs, no „wird durch Regel … geprüft“, no oscap as sentence
subject. Distill CaC `description`/`rationale` into German on-host behavior.

## Coverage matrix

One row per distinct aspect from statement + guidance.

| Guidance aspect | Covered by | Status |
|-----------------|------------|--------|
| Short German paraphrase | `rule_id` and/or doc URL | Yes / Partial / No |

**Example — BER.2.4:**

| Aspect | Covered by | Status |
|--------|------------|--------|
| Änderungen an Identitäts-Stammdaten protokolieren | `audit_rules_usergroup_modification_passwd` | Partial |
| Zeitpunkt im Ereignisprotokoll | auditd timestamp | Yes |
| Zugangskonto | audit UID/euid | Partial |
| Welche Änderungen | watch detects write, not field diff | Partial |
| IAM außerhalb lokaler Dateien | not OS-enforced | No — institution |

## CaC rule lookup

Used in Step 3 after `find-rule` returns IDs:

```text
{CAC_CONTENT_ROOT}/linux_os/guide/**/{rule_id}/rule.yml
```

Directory name = rule ID. Parent aggregates may list split rules in `warnings:` —
prefer the split rules in the CSV.

## Red Hat documentation sources

Order (SKILL Step 2):

1. **MCP** `user-Red-Hat-documentation` — `GetDynamicTools` first; one retry max
2. **`rhokp-docs`** (`../rhokp-scraper`) — scraped tree first, then search/scrape
3. **Hard stop** if RHOKP fails (missing, down, error, or no product-matching
   content). Ask user for **explicit permission** before any web search.
4. **WebSearch** (only with permission) — `site:docs.redhat.com` only (any page
   on that host). Verify each hit matches the component product + major
   (e.g. RHEL 9). Reject wrong product/version. Optional WebFetch of accepted URLs.

Never auto-fetch public web docs without that permission. Never invent URLs.

## Linking the source of truth

End prose with clickable public URLs (German label, max ~3):

```markdown
Weitere Informationen: [Audit-Aufzeichnungen konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/security_hardening/assembly_configuring-audit-records_security-hardening/), [Authentifizierung und Autorisierung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_authentication_and_authorization_in_rhel/index).
```

Always `docs.redhat.com` / `access.redhat.com` — never `localhost`.

- **MCP** — citation URL from the tool (confirm product/version)
- **RHOKP scraped md** — rewrite frontmatter `source_url`:
  `{RHOKP_BASE_URL}documentation/en-us/` → `https://docs.redhat.com/en/documentation/`
- **RHOKP KB** — public `access.redhat.com` URL
- **WebSearch hit** — use the `docs.redhat.com` URL only after product check
- **No doc reachable** — no link; note gap in PR body

Inline prose links are for humans reading the control md; they are separate
from any OSCAL `links` already on the IR.

## Protected markdown regions

```markdown
---
frontmatter: mostly from regenerate (rules/params); do not hand-edit Rule lists
---

# Title — UNCHANGED

## Control Statement — UNCHANGED

## Control guidance — UNCHANGED

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- comments — UNCHANGED -->

{EDIT: German prose — technical mechanism only; no CaC rule citations}

### Rules: — from regenerate (do not hand-edit)

### Implementation Status: {EDIT: value}

______________________________________________________________________
```

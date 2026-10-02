# Reference — enrich-component-implementation

## Implementation status

Evaluate against the **anticipated end state** — as if every strong CCE-backed
CaC rule from Step 3 (`find-rule`) will be attached on the IR in
`component-definitions/{COMPONENT_DEFINITION}/component-definition.json` — not
whether `### Rules:` currently shows them (usually empty at invoke). Apply in
order; first match wins unless a higher bar clearly fits.

### `implemented`

All must be true:

1. Every **technical** clause in Control Statement is addressed by strong Step 3
   CaC rules for the target RHEL major.
2. Every **technical** clause in Control guidance is addressed (as far as
   OS-level controls allow).
3. Those rules have `cce@rhel{N}`.
4. Covering rule IDs appear in the PR body (suggested CaC rules).
5. Prose describes the on-host mechanism — not the requirement, not rule IDs
   ([Implementation prose](#implementation-prose)).

### `partial`

Step 3 rules / docs cover only **part** of statement/guidance:

- Part of guidance only (e.g. `/etc/passwd` watch, not full IdM lifecycle)
- Audit lacks a field guidance asks for
- Product covers mechanism; institution covers process
- Control has organizational aspects (always at least `partial`)

Default when uncertain. Empty `### Rules:` / JSON does not change this — assume
strong discoveries will be attached.

### `alternative`

Genuinely **different** technical approach than literal statement/guidance
(same security intent). Not “no rule attached yet.” If a CaC rule targets the
literal mechanism, use `implemented`/`partial` even if JSON lacks `Rule_Id` yet.

### `planned`

No discovered rule **and** no doc-backed mechanism — not merely empty
`### Rules:`.

### `not-applicable`

Guidance is organizational-only; no defensible RHEL host hook. Do not use to
skip hard partial cases.

## Implementation prose

Prose under **What is the solution and how is it implemented?** = how RHEL
implements the control. CaC/OpenSCAP belong in JSON `Rule_Id`, PR body, and
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
use those as suggestions.

## Rule_Id and parameter values (JSON only)

`### Rules:` is **read-only display** (`trestle.common.const.RULES_WARNING`).
`trestle author component-assemble` does **not** write markdown rule bullets
into OSCAL. Source of truth:

| What | Where |
|------|--------|
| Attached rules | IR `props` with `"name": "Rule_Id"` |
| Chosen CaC var values | IR `set-parameters` (`param-id` + `values`) |
| Var metadata (optional) | `Parameter_Id`, `Parameter_Value_Alternatives` props |
| Prose / status | markdown → assemble → IR `description` / `implementation-status` |

Empty `x-trestle-param-values:` / `ber.X-prm1:` in markdown frontmatter are
**catalog control** placeholders (`{{ insert: param, … }}` in the statement),
not CaC rule variables. Leave them alone.

After JSON `Rule_Id` changes, `regenerate_components.sh` refreshes the markdown
`### Rules:` list from JSON.

**Develop CI order** (`check_and_update_all.sh`): if JSON changed → regenerate
(md from JSON); if markdown changed → assemble (JSON from md). Changing both in
one push without assembling locally first can wipe new prose. See SKILL Step 7.

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
frontmatter: UNCHANGED
---

# Title — UNCHANGED

## Control Statement — UNCHANGED

## Control guidance — UNCHANGED

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- comments — UNCHANGED -->

{EDIT: German prose — technical mechanism only; no CaC rule citations}

### Rules: — UNCHANGED (display from JSON Rule_Id)

### Implementation Status: {EDIT: value}

______________________________________________________________________
```

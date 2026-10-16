---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: package_sudo_installed
      description: package sudo installed
    - name: sudo_restrict_privilege_elevation_to_authorized
      description: sudo restrict privilege elevation to authorized
    - name: use_pam_wheel_for_su
      description: use pam wheel for su
    - name: no_direct_root_logins
      description: Direct root Logins Not Allowed
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# BER.3.6 - \[Zugangskonten\] Privilegierte Zugangskonten

## Control Statement

Berechtigung für Administrierende SOLLTE separate Zugangskonten für administrative Tätigkeiten (Administrationskonten) verankern.

## Control guidance

Zugangskonten mit privilegierten Rechten (Superuser wie z.B. root) könnten durch menschliche Fehler oder Schadcode weitreichende Probleme verursachen. Bewährt hat es sich daher für administrative Tätigkeiten wie die Installation von Anwendungen dedizierte Zugangskonten einzurichten und diese auch nur für derartige Tätigkeiten zu verwenden. Für normale Geschäftsaktivitäten wie E-Mail oder Webbrowser nutzen auch Administrierende dann ausschließlich Zugangskonten ohne administrative Berechtigungen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Typischerweise sind Accounts, die Zugriff auf einen RHEL-Host haben, bereits Administrative Konten. Die Ausnahme bilden hier RHEL-Clients. Es muss unter RHEL für nicht-administrative Konten sichergestellt sein, dass diese keinerlei `sudo`-Rechte erhalten, weder direkt noch über Gruppenzugehörigkeiten (bspw. `wheel`). Ob zusätzlich getrennte Personalkonten (Alltag vs. Administration) provisioniert werden, steuert die Institution in IdM/AD bzw. im lokalen Benutzerkonzept. Der Host erzwingt die organisatorische Trennung zweier Konten pro Person nicht selbst.

Weitere Informationen: [sudo-Zugriff verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-sudo-access_configuring-basic-system-settings), [Benutzer und Gruppen verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-users-and-groups_configuring-basic-system-settings).

### Rules:

  - no_direct_root_logins
  - package_sudo_installed
  - sudo_restrict_privilege_elevation_to_authorized
  - use_pam_wheel_for_su


### Implementation Status: partial

______________________________________________________________________

---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: directory_permissions_var_log_audit
      description: directory permissions var log audit
    - name: audit_rules_immutable
      description: audit rules immutable
    - name: file_permissions_var_log_audit
      description: file permissions var log audit
    - name: file_ownership_var_log_audit
      description: file ownership var log audit stig
    - name: file_group_ownership_var_log_audit
      description: file group ownership var log audit stig
    - name: directory_access_var_log_audit
      description: directory access var log audit
    - name: auditd_audispd_configure_remote_server
      description: auditd audispd configure remote server
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.3.5 - \[Protokollierung\] Revisionssicherheit

## Control Statement

Detektion SOLLTE Änderungen am Audit Log revisionssicher dokumentieren.

## Control guidance

Wenn die Protokollaufzeichnung unzureichend vor Veränderung geschützt ist, könnten Innentäter diese manipulieren oder löschen, um nicht erkannt oder belangt zu werden. Hierzu gehört auch, dass Administrierende die Protokolldaten zu ihren eigenen Tätigkeiten manipulieren oder löschen könnten. Die Integrität kann durch die Erstellung und getrennte Aufbewahrung von kryptografischen Hashes oder ein Versionskontrollsystem sichergestellt werden. Um sicherzustellen, dass nur autorisierte Personen die Protokolle verändern können, können z.B. Verschlüsselung und getrennte Aufbewahrung des Schlüssels, einmalig beschreibare Datenträger, oder ein Protokollierungsserver/SIEM mit stark eingeschränkten Zugriffsrechten eingesetzt werden. Auch die Aufzeichnung in einer öffentlichen Transparenzdatei ist möglich, wenn die Protokolle keine vertraulichen Daten enthalten.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Auf RHEL 9 schreibt auditd Audit-Ereignisse standardmäßig unter `/var/log/audit/`; die RHEL-Dokumentation empfiehlt restriktive Datei- und Verzeichnisrechte sowie Eigentümerschaft durch root, damit nur berechtigte Konten Protokolle lesen oder verändern können. Persistente Regeln in `/etc/audit/rules.d/` lassen sich mit einer Finalize-Regel (`-e 2`, Gruppe 90) unveränderlich machen, sodass Audit-Regeln bis zum Neustart nicht ohne Berechtigung angepasst werden können; Zugriffe auf Audit-Konfiguration und -Protokolle können zusätzlich per Audit-Regeln protokolliert werden. Für eine tatsächlich revisionssichere Ablage und zentrale Auswertung können Plugins unter `/etc/audit/plugins.d/` Ereignisse an einen entfernten Log- oder SIEM-Dienst weiterleiten — Transportverschlüsselung, getrennte Schlüsselhaltung, WORM-Speicher oder Hash-Ketten können dann im SIEM realisiert werden.

Weitere Informationen: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/auditing-the-system_security-hardening

### Rules:

  - directory_permissions_var_log_audit
  - audit_rules_immutable
  - file_permissions_var_log_audit
  - file_ownership_var_log_audit
  - file_group_ownership_var_log_audit
  - directory_access_var_log_audit
  - auditd_audispd_configure_remote_server

### Implementation Status: partial

______________________________________________________________________

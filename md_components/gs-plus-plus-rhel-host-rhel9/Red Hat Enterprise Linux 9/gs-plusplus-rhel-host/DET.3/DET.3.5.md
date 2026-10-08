---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: audit_rules_immutable
      description: audit rules immutable
    - name: file_permissions_var_log_audit
      description: file permissions var log audit
    - name: directory_permissions_var_log_audit
      description: directory permissions var log audit
    - name: file_ownership_var_log_audit_stig
      description: file ownership var log audit stig
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

<!-- Add control implementation description here for control: DET.3.5 -->

Auf RHEL 9 schreibt auditd Audit-Ereignisse standardmäßig unter `/var/log/audit/`; die RHEL-Dokumentation empfiehlt restriktive Datei- und Verzeichnisrechte sowie Eigentümerschaft durch root, damit nur berechtigte Konten Protokolle lesen oder verändern können. In `auditd.conf` kann `max_log_file_action` auf `keep_logs` gesetzt werden, damit rotierte Dateien nicht still überschrieben werden. Persistente Regeln in `/etc/audit/rules.d/` lassen sich mit einer Finalize-Regel (`-e 2`, Gruppe 90) unveränderlich machen, sodass Audit-Regeln bis zum Neustart nicht ohne Berechtigung angepasst werden können; Zugriffe auf Audit-Konfiguration und -Protokolle können zusätzlich per Audit-Regeln protokolliert werden. Für zentrale Auswertung können Plugins unter `/etc/audit/plugins.d/` Ereignisse an einen entfernten Log- oder SIEM-Dienst weiterleiten — Transportverschlüsselung, getrennte Schlüsselhaltung, WORM-Speicher oder Hash-Ketten bleiben organisatorisch zu betreiben.

Weitere Informationen: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/auditing-the-system_security-hardening

### Implementation Status: partial

______________________________________________________________________

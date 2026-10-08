---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_4_1
      description: Narrative implementation seed for DET.4.1 (no CaC rule 
        binding)
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.1 - \[Überwachung von Aktivitäten\] Überwachung der Protokollierung

## Control Statement

Detektion SOLLTE die Funktionsfähigkeit der Protokollierung überwachen.

## Control guidance

Zu den Kriterien kann beispielsweise die Aktivierung oder Deaktkvierung des Loggings auf Systemen, sowie die Datenmenge eingehender Logs in einem bestimmten Zeitraum gehören.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Die Überwachung, ob Protokollierung auf einem RHEL-Host noch aktiv ist und in erwarteter Menge anfällt, ist primär Aufgabe der Institution (SIEM, Log-Management, Betriebsüberwachung). RHEL stellt dafür Bausteine bereit: `systemd` verwaltet die Dienste `auditd`, `rsyslog` bzw. `systemd-journald`; deren Zustand lässt sich mit `systemctl` und über Journal-Einträge prüfen. Mit `journalctl` (lokal oder in der Web-Konsole unter Logs) können Administratoren Lücken, Fehler oder ungewöhnlich niedrige Aktivität erkennen; bei zentraler Weiterleitung (z. B. über `rsyslog` oder die Logging-RHEL-Systemrolle) lassen sich Schwellwerte und Alarme auf dem Log-Server oder in der Monitoring-Lösung definieren. Ein einzelner Host liefert keine fertige Erkennung „Logging deaktiviert“ oder „Logvolumen außerhalb Toleranz“ ohne zusätzliche Automatisierung oder zentrale Korrelation.

Weitere Informationen: [Probleme anhand von Logdateien analysieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/assembly_troubleshooting-problems-using-log-files_configuring-basic-system-settings), [Logging-RHEL-Systemrolle](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_using-the-logging-system-role_security-hardening), [Audit-Aufzeichnungen konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_configuring-audit-records_security-hardening)

### Implementation Status: partial

______________________________________________________________________

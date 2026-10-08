---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_3_1_6
      description: Narrative implementation seed for DET.3.1.6 (no CaC rule
        binding)
x-trestle-param-values:
  det.3.1.6-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.3.1.6 - \[Protokollierung\] Systemspezifische Ereignisse

## Control Statement

Detektion für IT-Systeme KANN {{ insert: param, det.3.1.6-prm1 }} protokollieren.

## Control guidance

Bestimmte systemspezifische Ereignisse meint hier, dass von der Instiution konkret festgehalten wurde, welche für das System relevanten Ereignisse im Einzelnen protokolliert werden. Beispiele sind Aktionen mit spezifisch konfigurierten privilegierten Berechtigungen, Prozessaktivitäten des Betriebssystems, wie das Starten eines Systemprozesses, Dateierzeugung oder das Laden eines Treibers, die Modifikation von Systemkonfigurationsdateien oder die Installation oder Deinstallation von Systemdiensten und Anwendungen, sowie das Herunterfahren oder Neustarten des Systems. Die Festlegung, welche dieser oder weiterer systemspezifischer Ereignisse protokolliert werden, obliegt der Institution und hängt von der jeweiligen Systemumgebung und dem Schutzbedarf ab.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Red Hat Enterprise Linux stellt mit dem Kernel-Audit-Subsystem (`auditd`) und dem systemd-Journal die technische Grundlage bereit, um vom Betriebssystem erzeugte Ereignisse zu protokollieren. Welche systemspezifischen Ereignisse für ein konkretes System relevant sind und damit erfasst werden sollen, legt die Institution fest. RHEL liefert für unterschiedliche Frameworks Beispiel-Regeln mit, die als Grundlage für die eigene Definition verwendet werden können.

Für die Umsetzung können Administratoren über `auditctl` sowie persistente Regeln unter `/etc/audit/rules.d/` (Auswertung mit `augenrules`) Syscalls, Dateizugriffe, Prozessstarts oder Konfigurationsänderungen gezielt aufzeichnen. Beispielregeln (z.B. für Common Criteria Protection Profile for General Purpose Operating Systems)) liegen in `/usr/share/audit/sample-rules`. Ereignisse landen standardmäßig in `/var/log/audit/audit.log` und sollten an die zentrale Log-Infrastruktur (SIEM) weitergegeben werden. Die Auswahl, Pflege und Prüfung der institutionsspezifischen Regelwerke sowie die operative Auswertung bleiben organisatorische Aufgaben.

`auditd` erfasst typischerweise nicht Aktionen wie das Installieren von Paketen, Neustarts oder generische Prozess-Starts. Diese werden allerdings bereits in der Standard-Konfiguration vom Betriebssystem protokolliert und in den entsprechenden Log-Files wie beispielsweise `/var/log/messages` abgelegt.

Weitere Informationen: [Audit-Aufzeichnungen konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/security_hardening/assembly_configuring-audit-records_security-hardening), [Überwachung und Verwaltung von Systemstatus und Performance](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/index)

### Rules:

  - gspp_impl_det_3_1_6

### Implementation Status: partial

______________________________________________________________________

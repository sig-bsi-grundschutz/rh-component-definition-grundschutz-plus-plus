---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_4_16
      description: Narrative implementation seed for DET.4.16 (no CaC rule 
        binding)
x-trestle-param-values:
  det.4.16-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.16 - \[Überwachung von Aktivitäten\] Ressourcenauslastung der Server-Dienste

## Control Statement

Detektion für Anwendungen KANN die Ressourcenauslastung der für die Anwendung verwendeten Server-Dienste anhand von {{ insert: param, det.4.16-prm1 }} überwachen.

## Control guidance

Hierzu zählt z.B. die Auslastung der CPU, des Arbeitsspeichers, des Festspeichers und Anzahl der verbundenen Clients. Dazu ist es sinnvoll vorab Schwellwerte zu ermitteln (KPI Baselining). Mögliche Reaktionsmaßnahmen bei zu hoher Auslastung sind z.B. die Lastverteilung auf mehrere Host-Rechner oder die Beschränkung der Ressourcennutzung pro Client.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Die Anforderung ist optional (KANN) und zielt auf die für Anwendungen genutzten Server-Dienste; auf RHEL 9 ordnet systemd jeden Dienst in der cgroup-Hierarchie ein, sodass CPU-, Arbeitsspeicher- und I/O-Auslastung pro Unit sichtbar wird (`CPUAccounting=`, `MemoryAccounting=`, `systemctl show`, `systemd-cgtop`). Grenzen und Reaktionen bei Überlast können in Unit-Dateien oder per `systemctl set-property` mit Optionen wie `CPUQuota=` oder `MemoryMax=` gesetzt werden. Performance Co-Pilot (PCP) mit `pmcd` und `pmlogger` erfasst Metriken fortlaufend; mit `pminfo`, `pmstat` oder dienstspezifischen PMDAs lassen sich Kennzahlen für KPI-Baselining und Schwellwertprüfungen auswerten, optional mit `pmie` für Alarme. Welche Metriken im Parameter `det.4.16-prm1` gelten, Baselines, Clientzahlen und Maßnahmen wie Lastverteilung definiert und betreibt die Institution; RHEL stellt die Werkzeuge, erzwingt aber keine anwendungsspezifische Überwachung.

Weitere Informationen: [Ressourcen mit systemd verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/assembly_using-systemd-to-manage-resources-used-by-applications_monitoring-and-managing-system-status-and-performance), [Performance Co-Pilot](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/monitoring-performance-with-performance-co-pilot_monitoring-and-managing-system-status-and-performance)

### Implementation Status: partial

______________________________________________________________________

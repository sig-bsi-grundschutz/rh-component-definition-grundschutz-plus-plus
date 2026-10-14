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

<!-- Add control implementation description here for control: DET.4.16 -->

RHEL stellt Werkzeuge zur Erfassung und Auswertung der Anwendungs-Ressourcenauslastung bereit, sofern diese über Standard-Systemfunktionen gestartet werden. Spezifische Anwendungen können spezifischere Anwendungs-Monitoring-Lösungen erfordern. Sofern das Pattern "Eine Anwendung - Ein Host" eingehalten wird, korreliert die Auslastung des Hosts regelmäßig mit der Last durch die Anwendung, mit der Ausnahme von Host-Prozessen. Mit dem Paket **sysstat** lassen sich CPU, Speicher und Platten-I/O fortlaufend protokollieren (`sadc`/`sar`, ergänzend `iostat` und `mpstat`). Der **Performance Co-Pilot (PCP)** mit `pmlogger` speichert Metriken historisch und unterstützt damit die  zentrale Auswertung. Mit `pminfo`, `pmstat` oder dienstspezifischen PMDAs lassen sich Kennzahlen für KPI-Baselining und Schwellwertprüfungen auswerten. Diese Informationen können durch ein zentrales Monitoring Tool (Nagios, Icinga und andere) aufgenommen oder alternativ über eigene Agenten erfasst werden. Für die kurzfristige Sicht beispielsweise bei Untersuchungen reichen Bordmittel wie `top`, `vmstat` und `df`.  Die automatische Alarmierung bei Schwellwertüberschreitung, KPI-Baselining und Maßnahmen wie Lastverteilung oder Ressourcenbegrenzung erfordern Monitoring-Backends oder Automatisierung außerhalb des RHEL-Hosts. Ebenfalls müssen institutionellen Schwellwerte und Reaktionsmaßnahmen durch  die Organisation festgelegt werden.

Weitere Informationen: [Überblick Performance-Monitoring (RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/overview-of-performance-monitoring-options), [Metriken in der Web-Konsole (RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/using-the-web-console-for-selecting-performance-profiles_monitoring-and-managing-system-status-and-performance), [How to Use SAR to Monitor System Performance in Red Hat Enterprise Linux](https://access.redhat.com/solutions/276533)

### Implementation Status: partial

______________________________________________________________________

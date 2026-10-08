---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_4_15
      description: Narrative implementation seed for DET.4.15 (no CaC rule 
        binding)
x-trestle-param-values:
  det.4.15-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.15 - \[Überwachung von Aktivitäten\] Ressourcenauslastung von Hostsystemen

## Control Statement

Detektion für Hostsysteme SOLLTE die Ressourcenauslastung anhand von {{ insert: param, det.4.15-prm1 }} überwachen.

## Control guidance

Hierzu zählt z.B. die Auslastung der CPU, des Arbeitsspeichers, des Festspeichers. Dazu ist es sinnvoll vorab Schwellwerte zu ermitteln (KPI Baselining). Mögliche Reaktionsmaßnahmen bei zu hoher Auslastung sind z.B. die Lastverteilung auf mehrere Host-Rechner oder die Beschränkung der Ressourcennutzung pro Client.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

<!-- Add control implementation description here for control: DET.4.15 -->

RHEL stellt Werkzeuge zur Erfassung und Auswertung der Host-Ressourcenauslastung bereit; die institutionellen Schwellwerte (Parameter „Schwellwerten“) und Reaktionsmaßnahmen legt die Organisation fest. Mit dem Paket **sysstat** lassen sich CPU, Speicher und Platten-I/O fortlaufend protokollieren (`sadc`/`sar`, ergänzend `iostat` und `mpstat`); **Performance Co-Pilot (PCP)** mit `pmlogger` speichert Metriken historisch und unterstützt zentrale Auswertung. In der **RHEL-Web-Konsole** (Paket `cockpit-pcp`, Dienste `pmlogger`/`pmproxy`) sind CPU-, Speicher- und Speicherplatz-Kennzahlen unter „Metrics and history“ einsehbar. Für kurzfristige Sicht reichen Bordmittel wie `top`, `vmstat` und `df`. Automatische Alarmierung bei Schwellwertüberschreitung, KPI-Baselining und Maßnahmen wie Lastverteilung oder Ressourcenbegrenzung (z. B. über cgroups/systemd) erfordern Betriebskonzepte, Monitoring-Backends oder Automatisierung außerhalb einer festen RHEL-Vorgabe.

Weitere Informationen: [Überblick Performance-Monitoring (RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/overview-of-performance-monitoring-options), [Metriken in der Web-Konsole (RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/using-the-web-console-for-selecting-performance-profiles_monitoring-and-managing-system-status-and-performance)

### Implementation Status: partial

______________________________________________________________________

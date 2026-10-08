---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_konf_9_2
      description: Narrative implementation seed for KONF.9.2 (no CaC rule 
        binding)
x-trestle-param-values:
  konf.9.2-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.9.2 - \[Verfügbarkeit von Ressourcen\] Begrenzung der Rechenleistung

## Control Statement

Konfiguration für Hostsysteme KANN die zur Verfügung stehende Rechenleistung anhand von {{ insert: param, konf.9.2-prm1 }} einschränken.

## Control guidance

Dies kann durch eine Beschränkung der Anzahl verwendeter Rechenkerne, der Rechenleistung pro Rechenkern oder durch eine indirekte Beschränkung (z.B. eine begrenzte Menge an Anfragen oder Eingabetoken in Anwendungen) umgesetzt werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Auf RHEL 9 steuert **systemd** die Rechenleistung von Diensten, Benutzer-Sessions und Slices über **cgroups v2**; typische Optionen in Unit-Dateien oder per `systemctl set-property` sind `CPUQuota=` (prozentualer CPU-Deckel), `CPUWeight=` (relative Gewichtung) und `AllowedCPUs=` (Kernbindung). Die im Profilparameter `konf.9.2-prm1` festgelegten Schwellwerte setzt die Institution optional pro Workload um, statt eine globale Standard-Obergrenze für den gesamten Host zu erzwingen. Indirekte Begrenzungen in Anwendungen (z. B. Request- oder Token-Limits) liegen außerhalb des Betriebssystems und erfordern zusätzliche Konfiguration der jeweiligen Software. Weitere Informationen: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/allocating-system-resources-using-systemd

### Implementation Status: alternative

______________________________________________________________________

---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_4_13
      description: Narrative implementation seed for DET.4.13 (no CaC rule 
        binding)
x-trestle-param-values:
  det.4.13-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.13 - \[Überwachung von Aktivitäten\] Verfügbarkeit des Hostsystems

## Control Statement

Detektion für Hostsysteme SOLLTE die Netzerreichbarkeit anhand von {{ insert: param, det.4.13-prm1 }} überwachen.

## Control guidance

Schwellwerte (engl. thresholds) sind hier Grenzwerte, die als Maßstab für die normale oder erwartete Netzerreichbarkeit des Hostsystems dienen. Diese Schwellwerte könnten beispielsweise eine bestimmte Anzahl an Fehlversuchen zur Erreichbarkeit in einem definierten Zeitfenster oder eine überdurchschnittlich hohe Anzahl an Verbindungsanfragen sein, die auf ungewöhnliche Netzwerkaktivität hindeuten. Ein Server könnte beispielsweise aufgrund eines Denial-of-Service-Angriffs (DoS) nicht mehr erreichbar sein, wodurch Dienste für Nutzende ausfallen. Ebenso könnte eine unerwartete Nichterreichbarkeit auf einen Hardwaredefekt, einen Konfigurationsfehler oder einen internen Angriff hindeuten, bei dem der Server vom Netz getrennt wurde, um Spuren zu verwischen. Die Überwachung anhand von Schwellwerten kann der Institution dabei helfen, solche Vorfälle frühzeitig zu erkennen und zu reagieren, bevor sie größeren Schaden anrichten. Die Überwachung kann über ein internes Monitoring-System umgesetzt werden, das kontinuierlich die Erreichbarkeit der Server mittels sogenannter Health-Checks oder Probes prüft. Dabei kann beispielsweise ein automatisches Ping-Verfahren eingesetzt werden, das in regelmäßigen Abständen die Antwortzeit des Servers misst. Die festgelegten Schwellwerte könnten zum Beispiel die maximal erlaubte Anzahl an aufeinanderfolgenden fehlgeschlagenen Ping-Antworten oder die durchschnittliche Antwortzeit sein.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Die Überwachung der Netzerreichbarkeit eines RHEL‑9‑Hosts anhand institutioneller Schwellwerte (z. B. Anzahl fehlgeschlagener Probes in einem Zeitfenster, maximale Antwortzeit) erfolgt in der Regel durch ein **externes** Monitoring-System (Nagios/Icinga, Prometheus mit Alertmanager, Grafana, Red Hat Advanced Cluster Management Observability oder vergleichbare Lösungen), das vom Netz aus Health-Checks gegen den Host ausführt (ICMP, TCP-Ports, HTTP(S)-Probes). RHEL setzt diese Schwellwerte nicht selbst; der Host kann Metriken und Lebenszeichen bereitstellen — etwa über Performance Co-Pilot (`pmcd`, optional Netzwerk-Performance-Domain-Agents) oder einen Prometheus-`node_exporter` — damit das zentrale System Ausfälle und Latenzen bewerten kann. Festlegung der Grenzwerte, Eskalation und Reaktion auf Alarme bleiben organisatorische Aufgaben der Institution.

Weitere Informationen: [Monitoring and managing system status and performance (RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/), [Observability (Red Hat Advanced Cluster Management)](https://docs.redhat.com/en/documentation/red_hat_advanced_cluster_management_for_kubernetes/2.11/html/observability/observability)

### Implementation Status: alternative

______________________________________________________________________

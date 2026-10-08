---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_5_10_3
      description: Narrative implementation seed for DET.5.10.3 (no CaC rule 
        binding)
x-trestle-param-values:
  det.5.10.3-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.5.10.3 - \[Management von Schwachstellen\] Automatisierte Überwachung von Anwendungsupdates

## Control Statement

Detektion für Anwendungen KANN den Patchstatus durch {{ insert: param, det.5.10.3-prm1 }} überwachen.

## Control guidance

Eine nicht gepatchte Anwendung könnte als Einfallstor für Angreifer dienen, die bekannte Schwachstellen ausnutzen, um sich Zugang zu Systemen oder Daten zu verschaffen. Die Umsetzung kann beispielsweise auf einem Patch Management System (PMS) oder einem Vulnerability Management System (VMS) basieren. Ein Patch-Managementsystem kann beispielsweise so konfiguriert werden, dass es kontinuierlich die Versionen der installierten Software mit einer zentralen Datenbank für verfügbare Updates abgleicht. Auch die Nutzung eines Schwachstellen-Scanners, der im Netzwerk nach ungepatchten Anwendungen sucht, ist eine wirksame Maßnahme. Ein solcher Scanner könnte beispielsweise wöchentlich oder sogar täglich einen Scan durchführen und die Ergebnisse in einem Dashboard visualisieren. Wichtige prozessuale Tipps sind die Einrichtung von Benachrichtigungsworkflows, die sicherstellen, dass kritische Patch-Status-Änderungen sofort an die richtigen Personen eskaliert werden, sowie die Integration der Überwachungsergebnisse in ein zentrales Incident Response System. Dies kann helfen, die Reaktionszeit zu verkürzen, sodass die Anwendungen schnellstmöglich aktualisiert werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL stellt kein integriertes Patch- oder Vulnerability-Management für beliebige Anwendungen bereit; der Parameter „det.5.10.3-prm1“ beschreibt die von der Institution gewählte Methode (z. B. PMS, VMS oder Schwachstellen-Scanner) und wird auf dem Host nicht automatisch durchgesetzt. Für containerisierte Anwendungen lassen sich mit Podman und Skopeo Image-Metadaten prüfen, Registry-Zugriffe steuern (`registries.conf`) und aktualisierte Images gezielt beziehen; wiederkehrende Abgleiche können über systemd-Timer oder Konfigurationsautomatisierung angebunden werden. OpenSCAP- und RHSA-OVAL-Inhalte adressieren primär von Red Hat ausgelieferte RPM-Pakete — für Drittanbieter- oder sprachspezifische Anwendungen sind zusätzliche, organisationsweite Prüfdefinitionen und Benachrichtigungsworkflows erforderlich. Auf registrierten Systemen kann Red Hat Lightspeed Hinweise zu Risiken und Empfehlungen liefern, ersetzt aber kein anwendungsspezifisches Patch-Monitoring.

Weitere Informationen: [Sicherheitshärtung — RHSA-OVAL und Drittanbieter-Software](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/scanning-the-system-for-security-compliance-and-vulnerabilities_security-hardening), [Container erstellen, ausführen und verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/building_running_and_managing_containers/index)

### Rules:

  - gspp_impl_det_5_10_3

### Implementation Status: partial

______________________________________________________________________

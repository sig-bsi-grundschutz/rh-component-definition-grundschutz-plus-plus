---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_asst_2_3_2
      description: Narrative implementation seed for ASST.2.3.2 (no CaC rule 
        binding)
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# ASST.2.3.2 - \[Inventarisierung\] Software Bill of Materials (SBOM)

## Control Statement

Informationen und Assets für Anwendungen SOLLTE die Software Bill of Materials (SBOM) dokumentieren.

## Control guidance

Eine Software Bill of Materials (SBOM) ist in diesem Zusammenhang eine strukturierte Liste aller Komponenten, Bibliotheken und Abhängigkeiten, die in einer Anwendung enthalten sind, einschließlich ihrer Versionen und Herkunft. Sie kann dabei sowohl Open-Source- als auch proprietäre Bestandteile erfassen und in maschinenlesbaren Formaten (z. B. SPDX, CycloneDX) vorliegen. Der Zweck dieser Dokumentation liegt darin, Transparenz über die eingesetzten Softwarebestandteile zu schaffen, sodass Abhängigkeiten, potenzielle Schwachstellen oder veraltete Komponenten nachvollziehbar bleiben. Ohne diese Transparenz könnte es bei Sicherheitsvorfällen, Lizenzkonflikten oder fehlender Wartbarkeit zu erheblichen Problemen kommen, während eine gepflegte SBOM die schnelle Identifikation von Risiken, die Minimierung von Vendor Lock-in und die Nachvollziehbarkeit der Software-Lieferkette unterstützen kann. Hierzu kann die BSI TR-03183-2 verwendet werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Auf dem RHEL-Host liefert die RPM-/DNF-Paketdatenbank eine maschinenlesbare Inventarliste installierter Betriebssystem- und Anwendungs-RPMs inklusive Name, Version, Architektur und Herkunft (Repository bzw. Signatur). Über `dnf list --installed`, `rpm -qa` bzw. `dnf repoquery --installed` sowie die Transaktionshistorie (`dnf history`) lässt sich der Host-Softwarestand nachvollziehen und bei Bedarf exportieren. Das entspricht einer hostseitigen Stückliste der ausgelieferten RPM-Komponenten, ersetzt aber keine anwendungsbezogene SBOM im Sinne von SPDX/CycloneDX inklusive transitiver Bibliotheksabhängigkeiten aus Build-Artefakten. Formale SBOMs für Applikationen, deren Pflege nach BSI TR-03183-2 und die Korrelation mit Schwachstellen-Advisories bleiben organisatorisch bzw. über Lieferketten-Werkzeuge (z. B. Build-Pipeline, Container-Image-SBOM, Satellite/Insights) zu etablieren.

Weitere Informationen: [Software mit dem DNF-Tool verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/index), [DNF-Befehle zum Auflisten von Paketen](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/assembly_yum-commands-list_managing-software-with-the-dnf-tool).


### Implementation Status: alternative

______________________________________________________________________

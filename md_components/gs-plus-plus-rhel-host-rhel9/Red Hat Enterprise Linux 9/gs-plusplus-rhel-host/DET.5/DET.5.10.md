---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: security_patches_up_to_date
      description: ensure software patches installed
    - name: package_dnf-automatic_installed
      description: install dnf-automatic package
    - name: timer_dnf-automatic_enabled
      description: enable dnf-automatic timer
    - name: dnf-automatic_apply_updates
      description: configure dnf-automatic to install available updates 
        automatically
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.5.10 - \[Management von Schwachstellen\] Zeitnahes Patchmanagement

## Control Statement

Detektion SOLLTE ein zeitnahes Patchmanagement verankern.

## Control guidance

Patches (Updates oder Sicherheitsaktualisierungen) sind neue Versionen, die Sicherheitslücken schließen. Je nach Aufbau der betroffenen Assets kann es bei der Aktualisierung auch erforderlich sein, Abhängigkeiten (Bibliotheken, Upstream Software) ebenfalls zu aktualisieren. Dies kann durch automatisierte Installation oder nach einem Test umgesetzt werden. Die Umsetzung kann auch den schrittweisen Rollout von Patches vorsehen, sodass bei Fehlern im Patch nicht alle Systeme gleichzeitig betroffen sind und auch komplexe Fehlerbilder durch Rückmeldungen frühzeitig erkannt werden können. Dies kann zum Beispiel nach dem One-Many-All-Prinzip oder Blue-Green-Deployment erfolgen. Zur Beurteilung der Kritikalität von Patches kann die Krititikalität der mit dem Patch verbundenen Schwachstellen, das Risikoprofil der zu patchenden Assets oder eine Korrelation komplexer Angriffswege herangezogen werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL bezieht Sicherheits- und Fehlerkorrekturen über die mit Subscription Manager konfigurierten Repositories. Mit `dnf update` bzw. `dnf update --security` und dem Abgleich verfügbarer Errata bleibt der Patchstand aktuell; `dnf-automatic` mit aktiviertem Timer kann Updates periodisch prüfen und optional automatisch einspielen (`apply_updates`). Stufenweise Ausrollung (One-Many-All, Blue/Green), Testfenster und Risikopriorisierung nach CVSS bzw. Asset-Kritikalität sind Organisationsprozesse (häufig über Satellite, Insights oder Change-Management) und werden vom einzelnen Host nicht allein erzwungen.

Weitere Informationen: [Automatisierung von Software-Updates in RHEL 9](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/assembly_automating-software-updates-in-rhel-9_managing-software-with-the-dnf-tool), [Installation von Sicherheitsupdates](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_and_monitoring_security_updates/installing-security-updates_managing-and-monitoring-security-updates).

### Rules:

  - security_patches_up_to_date
  - package_dnf-automatic_installed
  - timer_dnf-automatic_enabled
  - dnf-automatic_apply_updates


### Implementation Status: partial

______________________________________________________________________

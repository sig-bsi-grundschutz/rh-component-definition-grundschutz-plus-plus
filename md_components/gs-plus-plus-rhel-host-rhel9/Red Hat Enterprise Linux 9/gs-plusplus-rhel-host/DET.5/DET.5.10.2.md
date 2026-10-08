---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: security_patches_up_to_date
      description: ensure software patches installed
    - name: package_dnf-automatic_installed
      description: install dnf-automatic package
    - name: timer_dnf-automatic_enabled
      description: enable dnf-automatic timer
x-trestle-param-values:
  det.5.10.2-prm1:
    values:
      - einen automatisierten Mechanismus
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.5.10.2 - \[Management von Schwachstellen\] Automatisierte Überwachung von Systemupdates

## Control Statement

Detektion für IT-Systeme SOLLTE den Patchstatus durch {{ insert: param, det.5.10.2-prm1 }} überwachen.

## Control guidance

Der Patchsstatus des Informationsverbundes kann dabei durch Kennzahlen bestimmt werden, z.B. durchschnittliche Zeit bis zum Patch (Mean Time To Patch), Prozentsatz aktuell gepatchter Assets, Anzahl offener/geschlossener Ausnahmen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL stellt für die automatisierte Überwachung des Patchstatus auf dem Host die Paketverwaltung DNF bereit: Metadaten aus den konfigurierten Repositories zeigen verfügbare Sicherheits- und Bugfix-Errata (`dnf check-update`, `dnf updateinfo list security`). Für einen wiederkehrenden, systemgesteuerten Mechanismus ohne automatische Installation dient `dnf-automatic`: Nach Installation des Pakets wird in `/etc/dnf/automatic.conf` `apply_updates` deaktiviert gelassen; der systemd-Timer `dnf-automatic-notifyonly.timer` (oder ein konservativ konfigurierter `dnf-automatic.timer`) prüft periodisch auf Updates und meldet verfügbare Patches (z. B. per E-Mail), ohne Pakete zu installieren — technisch eng verwandt mit der Konfigurationsanforderung KONF.8.1, hier aus Detektionssicht für den lokalen Patchstatus. Ob der Stand den internen Vorgaben entspricht, bewertet die Institution zusätzlich (z. B. Errata-Abgleich, `dnf history`, OpenSCAP-Prüfungen oder zentrale Auswertung über Red Hat Satellite bzw. Red Hat Insights). Kennzahlen wie Mean Time To Patch oder Anteil gepatchter Systeme erfordern aggregierte Auswertung und Prozesse außerhalb des einzelnen Hosts.

Weitere Informationen: [Automatisierung von Software-Updates in RHEL 9](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/assembly_automating-software-updates-in-rhel-9_managing-software-with-the-dnf-tool), [Installation von Sicherheitsupdates](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_and_monitoring_security_updates/installing-security-updates_managing-and-monitoring-security-updates)

### Implementation Status: partial

______________________________________________________________________

---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: security_patches_up_to_date
      description: ensure software patches installed
    - name: package_dnf-automatic_installed
      description: install dnf-automatic package
    - name: timer_dnf-automatic_enabled
      description: enable dnf-automatic timer
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.8.1 - \[Sicherheitsupdates\] Automatische Überprüfung

## Control Statement

Konfiguration für IT-Systeme SOLLTE das Vorliegen von Sicherheitsupdates überwachen.

## Control guidance

Eine Überwachung von Sicherheitsupdates bedeutet, dass die IT-Systeme selbsttätig nach neuen Aktualisierungen suchen, die Schwachstellen in der Software beheben. Technisch können Systeme so konfiguriert werden, dass sie über zentrale Update-Server regelmäßig auf neue Patches prüfen. Es ist ratsam, einen automatisierten Prozess einzurichten, der bei Vorliegen von Updates diese automatisiert ausrollt oder eine Meldung an die zuständigen IT-Administratoren und ggf. die betroffenen Nutzer sendet. Diese Benachrichtigung kann über E-Mail, ein internes Ticketsystem oder ein Dashboard erfolgen. Ein guter Tipp ist die priorisierte Behandlung von Updates, bei der kritische Sicherheits-Patches vor Routine-Updates installiert werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Mit einer gültigen Red-Hat-Subscription stellt DNF über die konfigurierten Repositories Metadaten zu verfügbaren Aktualisierungen bereit; `dnf check-update` und `dnf updateinfo list security` zeigen an, ob Sicherheitskorrekturen für installierte Pakete vorliegen. Für eine wiederkehrende, systemgesteuerte Überwachung ohne automatische Installation wird das Paket `dnf-automatic` eingesetzt: In `/etc/dnf/automatic.conf` bleiben `apply_updates` deaktiviert und optional `upgrade_type = security` gesetzt; der systemd-Timer `dnf-automatic-notifyonly.timer` aktualisiert die Repository-Daten, meldet verfügbare Updates (z. B. per E-Mail oder Standardausgabe) und installiert keine Pakete. Alternativ kann `dnf-automatic.timer` mit entsprechend konservativer Konfiguration genutzt werden. Ob der Patchstand den internen Vorgaben entspricht, prüft die Institution zusätzlich periodisch (z. B. über `dnf history`, Errata-Abgleich oder zentrale Werkzeuge wie Red Hat Satellite bzw. Red Hat Insights). Benachrichtigung an Administratoren, Ticket-Workflows und priorisierte Ausroll-Prozesse sind organisatorisch festzulegen.

Weitere Informationen: [Automatisierung von Software-Updates in RHEL 9](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/assembly_automating-software-updates-in-rhel-9_managing-software-with-the-dnf-tool), [Installation von Sicherheitsupdates](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_and_monitoring_security_updates/installing-security-updates_managing-and-monitoring-security-updates)

### Implementation Status: partial

______________________________________________________________________

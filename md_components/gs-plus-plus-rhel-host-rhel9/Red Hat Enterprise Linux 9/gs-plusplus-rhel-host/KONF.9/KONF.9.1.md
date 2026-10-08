---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: mount_option_home_usrquota
      description: The usrquota mount option allows for the filesystem to have 
        disk quotas configured.
    - name: mount_option_home_grpquota
      description: The grpquota mount option allows for the filesystem to have 
        disk quotas configured.
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.9.1 - \[Verfügbarkeit von Ressourcen\] Speicherplatzbegrenzung

## Control Statement

Konfiguration für IT-Systeme SOLLTE den Speicherplatz für die Nutzerumgebung einschränken.

## Control guidance

Nutzerumgebung meint hier alle Anwendungen und Dienste, die auf dem System betrieben werden, aber keine Systemdienste sind. Alternativ empfiehlt es sich Mechanismen des verwendeten Datei- oder Betriebssystems zu nutzen, die Benutzende bei einem bestimmten Füllstand der Festplatte warnen oder nur noch Administrierenden Schreibrechte einräumen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Auf RHEL 9 werden Speicherkontingente für Nutzer- und Gruppenumgebungen typischerweise auf dem Dateisystem mit dem `quota`-Paket und XFS-Quotas umgesetzt: Für `/home` (oder andere Nutzerdaten-Partitionen) werden beim Mount die Optionen `usrquota` und `grpquota` gesetzt, anschließend `quotacheck` und `edquota` bzw. `xfs_quota` für Soft- und Hard-Limits auf Blöcke und Inodes. So kann der Verbrauch einzelner Konten begrenzt werden, bevor ein vollständiges Befüllen der Partition andere Anwendungen oder Systemdienste beeinträchtigt. Die Leitlinie empfiehlt zusätzlich Warnungen bei hohem Füllstand; das Quota-Subsystem meldet Überschreitungen der Soft-Limits, optional unterstützt der Dienst `quota_nld` Benachrichtigungen. Schreibschutz nur für Administratoren bei voller Platte (z. B. über `tmpfiles`, separate `/var`-Partitionen oder zentrale Speicher) ist betriebsspezifisch zu planen. Konkrete Kontingentwerte, betroffene Mountpoints und Überwachung (z. B. regelmäßiger `xfs_quota report`, Monitoring) legt die Institution fest.

Weitere Informationen: [Speichernutzung auf XFS mit Quotas begrenzen](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_storage_devices_and_file_systems/limiting-storage-space-usage-on-xfs-with-quotas_managing-file-systems), [Dateisysteme verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_storage_devices_and_file_systems/index)

### Rules:

  - mount_option_home_usrquota
  - mount_option_home_grpquota

### Implementation Status: partial

______________________________________________________________________

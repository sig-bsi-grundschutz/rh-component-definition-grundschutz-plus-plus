---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_konf_9_1
      description: Narrative implementation seed for KONF.9.1 (no CaC rule binding)
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

Für die Speicherplatzbegrenzung gibt es auf RHEL Systemen zwei Möglichkeiten. Möglichkeit eins ist die nutzung von quotas, beispielsweise bei Einsatz des `xfs` Dateisystems. Dies ermöglicht für spezifische Pfade (die der Anwendung entsprechend zugeordnet sind), Gruppen (`grpquota`) und Nutzer (`usrquota`)  Soft- und Hard-Quotas zu setzen. Eine Soft-Quota triggert eine entsprechende Warnung, die Hard-Quota verhindert weiteren Schreibzugriff. Sofern Filesysteme eingesetzt werden, die eine Quotierung nicht erlauben, besteht die Möglichkeit entsprechende Partitionen mittels `parted` oder logische Volumes mittels `lvm` zu verwenden. Hierdurch kann das Speichermedium unterhalb des Dateisystems in mehrere Bereiche aufgeteilt werden, wodurch der verfügbare Speicherplatz auf unterschiedliche Anwendungen oder Anwendungsteile aufgeteilt werden kann. Zu berücksichtigen ist bei beiden Methoden, dass beim Erreichen des Füllstandes von 100% die Funktion der Anwendung erheblich eingeschränkt sein, oder sogar Datenverlust auftreten kann. Konkrete Kontingentwerte, betroffene Mountpoints und Überwachung (z. B. regelmäßiger `xfs_quota report`, Monitoring) legt die Institution fest.

Weitere Informationen: [Speichernutzung auf XFS mit Quotas begrenzen](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_storage_devices_and_file_systems/limiting-storage-space-usage-on-xfs-with-quotas_managing-file-systems), [Dateisysteme verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_storage_devices_and_file_systems/index), [Partitionen mit parted verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_storage_devices/getting-started-with-partitions_managing-storage-devices), [Logische Volumes (LVM) konfigurieren und verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_logical_volumes/index)

### Implementation Status: partial

______________________________________________________________________

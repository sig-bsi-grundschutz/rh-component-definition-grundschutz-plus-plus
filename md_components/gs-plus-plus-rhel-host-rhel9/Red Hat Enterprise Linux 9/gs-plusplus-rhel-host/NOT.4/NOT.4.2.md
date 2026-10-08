---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_not_4_2
      description: Narrative implementation seed for NOT.4.2 (no CaC rule 
        binding)
x-trestle-param-values:
  not.4.2-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# NOT.4.2 - \[Datensicherung\] Sicherung des Systems

## Control Statement

Notfallplanung für IT-Systeme SOLLTE deren Datensicherung {{ insert: param, not.4.2-prm1 }} ausführen.

## Control guidance

Zu den erforderlichen Daten können z.B. Konfigurationsdateien des Betriebssystems, Firmware, Lizenzen, Treiber und die Systemdokumentation gehören. Bei gleichartigen Systemen kann die Anforderung auch durch die Sicherung einer Kopie erfolgen, wenn mit dieser alle IT-Systeme dieser Art funktionsfähig wiederhergestellt werden können. Die Anforderung kann auch durch die Wiederherstellung aus einem Versionskontrollsystem erfolgen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

<!-- Add control implementation description here for control: NOT.4.2 -->

Red Hat Enterprise Linux 9 enthält kein standardmäßig aktiviertes, zentral gesteuertes Backup-Produkt und keine feste Vorgabe für die Sicherungsfrequenz — diese legt die Institution im Notfallplan fest (Parameter `not.4.2-prm1`). Für die Datensicherung des Systems stehen Werkzeuge wie `tar`, `rsync`, Snapshots auf LVM oder Dateisystemebene, sowie optional Relax-and-Recover (ReaR) zur Erstellung bootfähiger Rescue-Images und Wiederherstellungsmedien bereit; ReaR muss installiert und in `/etc/rear/local.conf` mit passenden `BACKUP_URL`/`OUTPUT_URL` konfiguriert werden. Konfigurationsdateien, Zertifikate und dokumentierte Anpassungen sollten in den organisatorischen Backup-Umfang einbezogen werden; gleichartige Systeme können über ein Golden Image oder Versionsverwaltung abgedeckt werden. Überwachung, Aufbewahrung, Offsite-Kopien und Restore-Tests sind organisatorisch durchzuführen (z. B. mit Ansible, Satellite oder dedizierten Backup-Lösungen).

Weitere Informationen: [Sichern und Wiederherstellen des Systems](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/backing-up-and-restoring-your-system_configuring-basic-system-settings)

### Implementation Status: partial

______________________________________________________________________

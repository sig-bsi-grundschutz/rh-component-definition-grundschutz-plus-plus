---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_5_10_2
      description: Narrative implementation seed for DET.5.10.2 (no CaC rule 
        binding)
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

Für die automatisierte Überwachung des Patchstatus über mehrer RHEL-Hosts hinweg und im Kontext mit Informations-Verbund übergreifendem Schwachstellen-Management kann Red Hat Satellite als zentrale Informationsquelle für RHEL-Systeme dienen. Satellite hält den Patch-Stand registrierter Systeme fortlaufend nach und unterscheidet anwendbare von installierbaren Advisories (Security, Bugfix, Enhancement). Es lassen sich Patchstatus-Berichte für alle angebundenen RHEL-Hosts (regelmäßig) erzeugen und beispielsweise auch als CSV versenden. Eigene Report Templates können Errata-Anzahlen und -Typen je Lifecycle Environment aggregieren und so die notwendige Datenbasis für die Kennzahlen bereitstellen. Durch die Rohdaten lassen sich die Mean-Time-To-Patch und andere Kennwerte errechnen. Sollwerte, Ausnahmeregeln und Schwellen legt die Institution fest. Satellite stellt die fortlaufend aktualisierte Datengrundlage und die Auswertungsfunktionen bereit.

Weitere Informationen: [Using report templates to monitor hosts (Satellite 6.19)](https://docs.redhat.com/en/documentation/red_hat_satellite/6.19/html/managing_hosts/using_report_templates_to_monitor_hosts_managing-hosts), [Patching hosts through errata (Satellite 6.18)](https://docs.redhat.com/en/documentation/red_hat_satellite/6.18/html/managing_hosts/patching-hosts-through-errata)

### Implementation Status: partial

______________________________________________________________________

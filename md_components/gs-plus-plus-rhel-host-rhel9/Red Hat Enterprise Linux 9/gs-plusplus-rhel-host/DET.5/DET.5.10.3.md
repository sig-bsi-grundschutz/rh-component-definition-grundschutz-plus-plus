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

Anwendungen können auf unterschiedlichen Wegen installiert werden. Ein Weg über den die Red Hat Enterprise Linux Standard Tools den Prozess der automatisierten Überwachung der Updates unterstützen können ist, wenn die Anwendungen entweder auf durch Red Hat bereitgestellten Paketen basieren (z.B. `apache`, `nginx` oder verschiedene mitgelieferte Libraries, oder als `rpm`-Paket paketiert und bereitgestellt werden. In diesem Falle kann Red Hat Satellite wie in DET.5.10.2 beschrieben zur Verwaltung und Überwachung der Updates verwendet werden. Dies deckt jedoch nur die Teilstrecke "Repository" zu "Host-Systeme" ab, nicht den Weg vom Lieferanten in die Repositories oder bei selbstentwickelten Anwendungen die internen Anwendungsabhängigkeiten und deren Aktualisierung vor der `rpm`-Paketierung.

Weitere Informationen: [Software mit dem DNF-Tool verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/index).

### Implementation Status: partial

______________________________________________________________________

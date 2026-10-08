---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_4_10
      description: Narrative implementation seed for DET.4.10 (no CaC rule 
        binding)
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.10 - \[Überwachung von Aktivitäten\] Host-basierte Köder

## Control Statement

Detektion für IT-Systeme KANN Host-basierte Köder installieren.

## Control guidance

Köder sind Anwendungen, Dateien oder Datensätze auf dem IT-System, welche die Aufmerksamkeit von Angreifern auf sich ziehen, um diese zu entdecken, nachzuverfolgen oder von echten Zielen abzulenken. Sie werden auch als Canaries oder Tripwire bezeichnet. Beispielsweise kann das Sicherheitsteam eine gefälschte, aber verlockende Datei (z. B. „IBAN-Kontodaten.xlsx“) im System platzieren und eine Überwachung einrichten, die sie benachrichtigt, wenn die Datei berührt wird - da legitime Benutzer nicht darauf zugreifen können, signalisiert jede Interaktion potenziell unbefugte Aktivitäten. Ein weiteres Beispiel ist eine Datei „unattended.xml“, da sie für Angreifer nützliche Anmeldedaten für automatische Installationen enthalten könnte. Indem Sie eine gefälschte Version mit harmlosen Daten erstellen und den Zugriff auf die Datei oder Anmeldeversuche mit diesen Zugangsdaten überwachen, erhalten Sie eine frühzeitige Warnung, wenn jemand Ihr System auf der Suche nach einfachen Möglichkeiten zur Erlangung von Administratorrechten durchforstet, so dass Sie reagieren können, bevor es zu einem schwerwiegenderen Verstoß kommt. Allerdings kann es hierbei zu falsch-positiv Vorfallsmeldungen kommen, insbesondere wenn die Köder dort platziert werden wo sie für legitime Nutzende leicht zugänglich sind.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

<!-- Add control implementation description here for control: DET.4.10 -->

Red Hat Enterprise Linux 9 liefert kein integriertes Subsystem für hostbasierte Köder (Canary-Dateien, Honeytokens oder täuschende Dienste). Die Anforderung ist optional („KANN“) und beschreibt ein betriebliches Deception-Konzept: Köder platzieren, Zugriffe erkennen, Alarme auslösen und Falschmeldungen vermeiden — das obliegt dem Sicherheitsteam und liegt nicht im Standardumfang des Betriebssystems. Mit **auditd** können Institutionen bei selbst angelegten Köderdateien Zugriffe protokollieren und auswerten; RHEL stellt dafür keine vorgefertigten Köder, keine zentrale Verwaltung und keine SIEM-Anbindung bereit. Spezialisierte Honeypot- oder Deception-Produkte sind separate Beschaffungs- und Betriebsentscheidungen.

Weitere Informationen: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/auditing-the-system_security-hardening

### Implementation Status: not-applicable

______________________________________________________________________

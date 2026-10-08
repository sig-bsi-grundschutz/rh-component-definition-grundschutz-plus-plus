---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_4_9
      description: Narrative implementation seed for DET.4.9 (no CaC rule 
        binding)
x-trestle-param-values:
  det.4.9-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.9 - \[Überwachung von Aktivitäten\] Manipulations-Checkup

## Control Statement

Detektion für IT-Systeme KANN das System auf Manipulationsversuche {{ insert: param, det.4.9-prm1 }} überprüfen.

## Control guidance

Falls Systeme einem erhöhten Manipulationsrisiko ausgesetzt sind (z.B. wegen öffentlicher Aufstellung), die Vertraulichkeit oder Integrität des Systems oder damit verbundener Daten oder Netze jedoch nicht vernachlässigenswert ist, so ist eine regelmäßige Überprüfung auf Manipulationen empfehlenswert. Hierfür können Gerätesiegel verwendet werden. Maßnahmen bei Feststellung einer Manipulation können z.B. das Zurücksetzen auf den Werkszustand oder die Aussonderung sein.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Die Anforderung ist optional (KANN) und betrifft vor allem Systeme mit erhöhtem Manipulationsrisiko. RHEL kann bei organisatorischer Freigabe regelmäßige Prüfungen auf unautorisierte Änderungen am Dateisystem mit AIDE (Advanced Intrusion Detection Environment) durchführen: Nach Initialisierung einer Referenzdatenbank vergleicht `aide --check` konfigurierte Pfade mit der Baseline; Abweichungen weisen auf mögliche Manipulationen hin. Die Prüfung lässt sich per Cron oder systemd-Timer planen und die Ergebnisse per E-Mail oder zentrales Monitoring auswerten. Ergänzend kann die Integrity Measurement Architecture (IMA) Integrität einzelner Dateien über Messwerte in erweiterten Attributen absichern. Physische Schutzmaßnahmen wie Gerätesiegel sowie Maßnahmen nach Feststellung einer Manipulation (z. B. Werksreset oder Aussonderung) sind organisatorisch festzulegen und werden vom Betriebssystem nicht automatisch ausgelöst.

Weitere Informationen: [Integritätsprüfung mit AIDE](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/checking-integrity-with-aide_security-hardening)

### Rules:

  - gspp_impl_det_4_9

### Implementation Status: partial

______________________________________________________________________

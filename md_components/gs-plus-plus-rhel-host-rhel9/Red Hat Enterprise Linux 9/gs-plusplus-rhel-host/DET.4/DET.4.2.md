---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: aide_build_database
      description: aide build database
    - name: aide_periodic_cron_checking
      description: aide periodic cron checking
    - name: aide_scan_notification
      description: aide scan notification
    - name: package_aide_installed
      description: package aide installed
    - name: install_hids
      description: install intrusion detection software
x-trestle-param-values:
  det.4.2-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.2 - \[Überwachung von Aktivitäten\] Automatische Angriffserkennung

## Control Statement

Detektion für IT-Systeme SOLLTE diese auf Anzeichen für Angriffe durch {{ insert: param, det.4.2-prm1 }} überwachen.

## Control guidance

Wenn professionelle Tätergruppen Zugriff auf Systeme und Daten erhalten, nutzen sie diese zunehmend schneller für ihre Zwecke aus, z.B. um Daten abfließen zu lassen oder Ransomware zu verteilen. Zur Umsetzung können sowohl netz- als auch hostbasierte Erkennungssysteme (NIDS und HIDS) verwendet werden. Für die Detektion bei IT-Systemen ohne Installationsmöglichkeit wie Appliances, IoT-Geräte oder OT-Systeme kann ein kombinierter Ansatz aus Netzwerk- und Loganalyse sinnvoll sein. Angriffe können signaturbasiert, sowie durch Verhaltensanalyse und Anomalien erkannt werden. Die Anforderung kann auch mit bereits vorhandenen oder im System integrierten Angriffserkennungsmechanismen erfüllt werden. Zweckmäßig ist es Schwellwerte und Kategorien (Info, Warnung, Alarm) so festzulegen, dass Probleme frühzeitig erkannt werden können, aber beim Betriebspersonal keine Alarmmüdigkeit (alert fatigue) aufkommt. Hierzu ist es hilfreich die Ergebnisse regelmäßig auszuwerten und wenn nötig Korrekturmaßnahmen zu ergreifen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL stellt hostbasierte Bausteine zur Erkennung von Angriffsindikatoren bereit, die die Institution über den Profilparameter für den automatisierten Überwachungsmechanismus konkretisiert. Advanced Intrusion Detection Environment (AIDE) erstellt eine Referenzdatenbank kritischer Dateien und vergleicht diese periodisch (Cron oder systemd-Timer); Abweichungen weisen auf unautorisierte Manipulationen hin und können per Benachrichtigung an Betrieb oder SOC eskaliert werden. Damit deckt AIDE Integritätsverletzungen ab, nicht signatur- oder verhaltensbasierte Angriffsmuster im Sinne eines vollständigen HIDS.

Zusätzlich bringt die Plattform ein Audit-Subsystem (`auditd`) und SELinux-Mandatory Access Control mit, die Aktivitäten protokollieren bzw. eindämmen und in Verbindung mit institutionellen Audit-Regeln sowie zentraler Auswertung (SIEM, Korrelationsjobs) Anzeichen für Kompromittierung sichtbar machen können. Netzwerkbasierte IDS, dedizierte kommerzielle HIDS-Agenten sowie Auswertung, Schwellwerte und Alarmkategorien liegen in der Verantwortung der Institution.

Weitere Informationen: [Dateiintegrität mit AIDE prüfen](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_checking-file-integrity-with-aide_security-hardening), [Audit-Aufzeichnungen konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/security_hardening/configuring-audit-records_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Implementation Status: partial

______________________________________________________________________

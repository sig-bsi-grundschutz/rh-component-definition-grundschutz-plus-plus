---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: package_usbguard_installed
      description: package usbguard installed
    - name: service_usbguard_enabled
      description: service usbguard enabled
    - name: usbguard_generate_policy
      description: usbguard generate policy
    - name: configure_usbguard_auditbackend
      description: Log USBGuard daemon audit events using Linux Audit
---

# DET.3.1.3 - \[Protokollierung\] Anbindung von Peripheriegeräten

## Control Statement

Detektion für IT-Systeme SOLLTE das Anschließen von Peripheriegeräten protokollieren.

## Control guidance

Das Protokollieren der Anbindung von Peripheriegeräten kann helfen, Manipulationsversuche an IT-Systemen frühzeitig zu erkennen und nachzuvollziehen. Ohne ein solches Protokoll könnte beispielsweise ein unbefugtes Speichermedium angeschlossen und vertrauliche Daten unbemerkt entwendet werden, oder es könnte Schadsoftware über ein USB-Gerät eingeschleust werden. Auch manipulierte Eingabegeräte könnten genutzt werden, um Tastatureingaben auszulesen oder unbemerkt Befehle einzuschleusen. Unter Peripheriegeräten sind in diesem Kontext externe Komponenten (aus Hardware oder virtuell) zu verstehen, die ein IT-System erweitern oder mit diesem verbunden werden – etwa USB-Sticks, externe Festplatten, Smartphones im Lade- oder Datenmodus, Drucker oder auch spezialisierte Geräte wie Diagnose- oder Messinstrumente. Zur praktischen Umsetzung kann eine Institution beispielsweise auf Betriebssystemfunktionen zurückgreifen, die Geräteanschlüsse im System-Log erfassen, oder ergänzende Endpoint-Management-Lösungen einsetzen, die eine zentralisierte Protokollierung erlauben.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL kann das Anschließen und die Autorisierung von USB-Peripherie über USBGuard steuern und nachvollziehbar machen. Nach Installation und Aktivierung des Dienstes erkennt USBGuard angeschlossene Geräte. Eine in `/etc/usbguard/rules.conf` hinterlegte Policy legt fest, welche Geräte zugelassen oder blockiert werden. Nicht autorisierte Anschlüsse werden damit unterbunden, bevor sie das System nutzen können. Für die Protokollierung sollte in `/etc/usbguard/usbguard-daemon.conf` die Audit-Schnittstelle auf Linux Audit (`AuditBackend=LinuxAudit`) gestellt werden, damit Autorisierungsereignisse zentral im Audit-Log landen, auswertbar sind und von dort an ein zentrales SIEM weitergeleitet werden können. Andere Peripherieklassen (z. B. einige virtuelle oder nicht-USB-Schnittstellen) sowie zentrale SIEM-Auswertung und organisatorische Freigabeprozesse bleiben Aufgabe der Institution.

Weitere Informationen: [Schutz vor aufdringlichen USB-Geräten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/protecting-systems-against-intrusive-usb-devices_security-hardening)

### Rules:

  - configure_usbguard_auditbackend
  - package_usbguard_installed
  - service_usbguard_enabled
  - usbguard_generate_policy

### Implementation Status: partial

______________________________________________________________________

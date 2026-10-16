---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: service_auditd_enabled
      description: service auditd enabled
    - name: audit_rules_mac_modification
      description: audit rules mac modification
    - name: audit_rules_privileged_commands
      description: Ensure auditd Collects Information on the Use of Privileged 
        Commands
    - name: audit_sudo_log_events
      description: Record Attempts to perform maintenance activities
    - name: audit_rules_sysadmin_actions
      description: Ensure auditd Collects System Administrator Actions
    - name: audit_rules_networkconfig_modification
      description: Record Events that Modify the System's Network Environment
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.3.1.8 - \[Protokollierung\] Privilegierte Ereignisse

## Control Statement

Detektion für Anwendungen SOLLTE privilegierte Ereignisse einschließlich der Aktivierung, Deaktivierung oder Blockierung privilegierter Funktionen protokollieren.

## Control guidance

Privilegierte Ereignisse sind Vorgänge, bei denen besonders weitreichende Rechte genutzt werden – beispielsweise die Vergabe oder Entziehung von Administratorrechten, das Deaktivieren von Virenscannern oder Änderungen an Firewallregeln. Gerade solche Eingriffe könnten einen erheblichen Einfluss auf die Verfügbarkeit und Integrität von Daten haben. Ohne eine gezielte Aufzeichnung könnten sicherheitsrelevante Änderungen unentdeckt bleiben – etwa, wenn ein Angreifer unbefugt einen privilegierten Account übernimmt und Spuren verwischt, oder wenn ein interner Benutzer kritische Funktionen deaktiviert, wodurch Schutzmaßnahmen umgangen werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Mit aktiviertem `auditd` protokolliert RHEL privilegierte Vorgänge auf dem Host: Ausführung setuid-/privilegierter Befehle, sudo-/Wartungsaktionen, Änderungen an SELinux-Policy und Netzwerkkonfiguration sowie weitere Administrator-Ereignisse. Zeitstempel, Akteure (UID/`auid`) und betroffene Objekte erscheinen in den Audit-Aufzeichnungen und unterstützen die Nachvollziehbarkeit von Aktivierung, Deaktivierung oder Umgehung schützender Funktionen. Welche Anwendungsschicht-Funktionen darüber hinaus als „privilegiert“ gelten und zentral korreliert werden, bleibt organisatorisch bzw. über SIEM zu definieren.

Weitere Informationen: [Audit-Aufzeichnungen konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/security_hardening/assembly_configuring-audit-records_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Rules:

  - audit_rules_privileged_commands
  - audit_sudo_log_events
  - audit_rules_sysadmin_actions
  - audit_rules_networkconfig_modification
  - service_auditd_enabled
  - audit_rules_mac_modification


### Implementation Status: partial

______________________________________________________________________

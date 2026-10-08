---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: audit_rules_mac_modification
      description: audit rules mac modification
    - name: audit_rules_sudoers
      description: audit rules sudoers
    - name: audit_rules_sudoers_d
      description: audit rules sudoers d
    - name: audit_rules_usergroup_modification_pamd
      description: audit rules usergroup modification pamd
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.4 - \[Überwachung von Aktivitäten\] Änderungen an Sicherheitsrichtlinien

## Control Statement

Detektion SOLLTE Änderungen an Sicherheitsrichtlinien einschließlich deren Aktivierung oder Deaktivierung überwachen.

## Control guidance

Wird die Aktivität von automatisierten Sicherheitswerkzeugen nicht überwacht, so könnten Angreifer diese Schutzmechanismen unbemerkt deaktivieren und die Person so in falscher Sicherheit wiegen. Zudem installieren Angreifer gerne permanente Hintertüren über neue Konten oder Gruppenwechsel. Automatisierte Sicherheitsrichtlinien sind z.B. Ausnahmelisten von Antivirus- oder EDR, über den Verzeichnisdienst hinzugefügte Gruppenzugehörigkeiten zu sicherheitsrelevanten Gruppen (z.B. Admin), NAC oder Firewallregeln.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL protokolliert mit auditd über persistente Dateiüberwachungsregeln Schreib- und Attributänderungen an zentralen Sicherheitsrichtlinien: unter anderem SELinux-Konfiguration unter `/etc/selinux/`, sudoers-Regeln in `/etc/sudoers` und `/etc/sudoers.d/` sowie PAM-Stack-Dateien in `/etc/pam.d/`. Die Ereignisse stehen in den Audit-Logs zur forensischen Auswertung bereit. Änderungen an weiteren Richtlinien (z. B. SSH- oder Firewall-Konfiguration) sowie das unbemerkte Stoppen von Schutzdiensten erfordern zusätzliche, institutionell festgelegte Regeln, zentrale Sammlung und Alarmierung; reine Host-Härtung deckt EDR-Ausnahmen, Verzeichnisdienst-Gruppen oder NAC nicht ab.

Weitere Informationen: [Auditing the system](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/auditing-the-system_security-hardening)

### Implementation Status: partial

______________________________________________________________________

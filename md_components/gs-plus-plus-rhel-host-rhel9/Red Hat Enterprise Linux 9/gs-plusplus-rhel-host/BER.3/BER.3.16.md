---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: account_unique_id
      description: account unique id
    - name: accounts_authorized_local_users
      description: accounts authorized local users
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# BER.3.16 - \[Zugangskonten\] Keine Gruppenkonten

## Control Statement

Berechtigung SOLLTE Gruppenkonten im Regelbetrieb untersagen.

## Control guidance

Ein Gruppenkonto (englisch shared account) bezeichnet hier ein Zugangskonto, das von mehreren natürlichen Personen gemeinsam genutzt wird und daher keiner einzelnen natürlichen Person eindeutig zugeordnet werden kann. Vom Regelbetrieb erfasst ist die gewöhnliche, wiederkehrende Nutzung im Tagesgeschäft, nicht jedoch ein eng begrenzter Ausnahmefall mit gesonderter Begründung und Absicherung, sowie nachvollziehbarer Zuordnung der Nutzung zu einer konkreten Person. Der Ausschluss von Gruppenkonten hat das Ziel die Nachvollziehbarkeit von Zugriffen und Änderungen zu verbessern und eine eindeutige Verantwortlichkeit für die Nutzung von Berechtigungen sicherzustellen. Die Verwendung von Gruppenkonten im Regelbetrieb könnten hingegen die Aufklärung von Sicherheitsvorfällen erschweren, die missbräuchliche Nutzung von Zugriffsrechten begünstigen oder nach dem Ausscheiden einzelner Nutzer unbemerkte Zugriffsmöglichkeiten bestehen lassen. Für technisch unvermeidbare Sonderfälle können stattdessen speziell gekennzeichneter Funktions- oder Dienstkonten (service accounts) verwendet werden. Im Unterschied zu Gruppenkonten sind Service Accounts nicht zur gemeinsamen interaktiven Nutzung durch mehrere Mitarbeiter gedacht. Sie können beispielsweise einen Datenbankdienst mit einer Anwendung verbinden, automatisierte Datensicherungen durchführen oder den Datenaustausch zwischen zwei Systemen ermöglichen. Um klar von Gruppenkonten abgegrenzt zu sein benötigt ein Dienstkonto eine klar nachvollziehbare Zweckbindung, die Sperrung interaktiver Anmeldungen, sowie eine regelmäßige Erneuerung von Authentisierungsmerkmalen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Gruppenkonten im Sinne gemeinsam genutzter interaktiver Logins mehrerer Personen verhindert das Betriebssystem nicht automatisch — das Verbot shared accounts im Regelbetrieb und die Zweckbindung von Dienstkonten (keine interaktive Anmeldung, klare Ownership) sind Provisionierungs- und IdM-/IAM-Vorgaben der Institution. Bei der Nutzung von Service-Accounts wird durch das setzten einer gesperrten Shell (`nologin`/`false`) der Missbrauch des Accounts als interaktives Gruppenkonto unterbunden.

Weitere Informationen: [Benutzer und Gruppen verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-users-and-groups_configuring-basic-system-settings), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Rules:

  - account_unique_id
  - accounts_authorized_local_users

### Implementation Status: partial

______________________________________________________________________

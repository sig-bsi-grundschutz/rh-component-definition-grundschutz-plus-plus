---
x-trestle-param-values:
  ber.3.14-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: account_use_centralized_automated_auth
      description: account use centralized automated auth
---

# BER.3.14 - \[Zugangskonten\] Kein Recycling von Zugängen

## Control Statement

Berechtigung SOLLTE die Wiederverwendung von Zugangskonten für {{ insert: param, ber.3.14-prm1 }} blockieren.

## Control guidance

Wiederverwendung von Zugangskonten meint hier die erneute Vergabe oder Reaktivierung zuvor bereits verwendeter Zugangskonten für Einzelpersonen, Gruppen, Rollen, Dienste oder Geräte zu anderen Einzelpersonen, Gruppen, Rollen, Diensten oder Geräten (engl. account reuse, account recycling). Der Parameter „einen bestimmten Zeitraum“ beschreibt eine durch die Institution festgelegte Sperr- oder Karenzfrist, innerhalb derer ein deaktiviertes, entzogenes oder nicht mehr zugeordnetes Konto nicht erneut verwendet werden kann; sinnvolle Werte können je nach Schutzbedarf etwa 90 Tage, 180 Tage, ein Jahr oder bei besonders kritischen bzw. privilegierten Konten eine dauerhafte Nichtwiederverwendung sein. Ohne eine solche Sperrfrist könnte eine neue nutzende Person fälschlich Zugriff auf alte Berechtigungen, Protokollzuordnungen, Postfächer, Schlüssel, Tokens oder Anwendungskontexte erhalten, und ein Sicherheitsvorfall könnte später nicht mehr eindeutig einer handelnden Person oder einem technischen Vorgang zugeordnet werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL erzwingt lokal keine automatische Sperr- oder Karenzfrist gegen das Recycling von Zugangskontennamen oder UIDs: Nach `userdel` können Name und UID sofort erneut vergeben werden. Mit SSSD an Red Hat IdM, Active Directory oder LDAP angebundene Hosts beziehen Konten aus dem Verzeichnis; dort lässt sich Wiederverwendung steuern — in IdM etwa über den Lebenszyklus *preserve* (`ipa user-del --preserve`), der den Kontonamen belegt hält, statt ihn dauerhaft freizugeben. Lokal ist die Alternative, Konten nicht zu löschen, sondern mit `usermod -L` zu sperren bzw. über `chage -E` zu befristen und so Name und UID während der Karenzfrist zu reservieren; über mehrere Hosts hinweg gehört das in die zentrale Kontoverwaltung bzw. Konfigurationsautomatisierung. Die institutionelle Frist selbst (90 Tage, ein Jahr, dauerhafte Nichtwiederverwendung) prüft der einzelne RHEL-Host nicht.

Weitere Informationen: [Benutzerkonten in IdM verwalten (CLI)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_idm_users_groups_hosts_and_access_control_rules/managing-user-accounts-using-the-command-line_managing-users-groups-hosts), [Benutzer und Gruppen verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-users-and-groups_configuring-basic-system-settings), [Authentifizierung und Autorisierung (SSSD)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_authentication_and_authorization_in_rhel/understanding-sssd-and-its-benefits_configuring-authentication-and-authorization-in-rhel)

### Rules:

  - account_use_centralized_automated_auth

### Implementation Status: alternative

______________________________________________________________________

---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_ber_4_5
      description: Narrative implementation seed for BER.4.5 (no CaC rule
        binding)
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# BER.4.5 - \[Berechtigungsmanagement\] Systemfunktionen ohne Authentifizierung

## Control Statement

Berechtigung für IT-Systeme SOLLTE Funktionen, auf die ohne vorherige Authentifizierung zugegriffen werden kann, dokumentieren.

## Control guidance

Funktionen ohne Authentifizierung sind alle Zugriffsmöglichkeiten auf Schnittstellen oder Daten des Systems, für die keine Authentifizierung erforderlich ist. Hierzu gehören z. B. Sprachassistenten, offene Webserver-Ports oder das Einblenden von Inhalten aus Apps auf dem Sperrbildschirm, wodurch persönliche Nachrichten oder Logintoken für Unbefugte zugänglich sein könnten. Solche Funktionen sind häufige Einfallstore für Angriffe auf das System oder für Datenleaks. Daher ist es sinnvoll eine Übersicht dieser Funktionen zu führen, selbst wenn die Funktionen benötigt werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Bei einer RHEL-Installation ohne grafische Oberfläche (z. B. Minimal Install oder Server; die Anaconda-Standardauswahl ist hingegen „Server with GUI“) gibt `firewalld` in der Standardzone ausschließlich den Service `ssh` (TCP-Port 22) frei — der Zugriff über SSH erfordert Authentifizierung; passwortbasiertes Root-Login per SSH ist standardmäßig deaktiviert. Bei grafischer Oberfläche ist sicherzustellen, dass kein automatischer Login (GDM) aktiviert ist — dies ist jedoch keine Anaconda-Installer-Option, sondern nachträgliche Konfiguration. Während der Installation muss ein Root-Passwort gesetzt werden. Ebenfalls ist die Anlage eines lokalen Benutzerkontos oder die Integration mit einem zentralen IdP (Red Hat IdM, Active Directory, LDAP) möglich.

Weitere Informationen: [System im Installer anpassen](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/interactively_installing_rhel_from_installation_media/customizing-the-system-in-the-installer_rhel-installer)

### Implementation Status: alternative

______________________________________________________________________

---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: accounts_password_pam_dictcheck
      description: accounts password pam dictcheck
    - name: accounts_password_pam_maxrepeat
      description: accounts password pam maxrepeat
    - name: accounts_password_pam_maxsequence
      description: accounts password pam maxsequence
    - name: accounts_password_pam_pwquality_password_auth
      description: accounts password pam pwquality password auth
    - name: accounts_password_pam_pwquality_system_auth
      description: accounts password pam pwquality system auth
    - name: package_pam_pwquality_installed
      description: package pam pwquality installed
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_password_pam_dictcheck
      description: var password pam dictcheck
      options: '1'
      rule-id: accounts_password_pam_dictcheck
    - name: var_password_pam_maxrepeat
      description: var password pam maxrepeat
      options: '3'
      rule-id: accounts_password_pam_maxrepeat
    - name: var_password_pam_maxsequence
      description: var password pam maxsequence
      options: '3'
      rule-id: accounts_password_pam_maxsequence
x-trestle-comp-def-rules-param-vals:
  # You may set new values for rule parameters by adding
  #
  # component-values:
  #   - value 1
  #   - value 2
  #
  # below a section of values:
  # The values list refers to the values as set by the components, and the component-values are the new values
  # to be placed in SetParameters of the component definition.
  #
  Red Hat Enterprise Linux 9:
    - name: var_password_pam_dictcheck
      values:
        - '1'
    - name: var_password_pam_maxrepeat
      values:
        - '3'
    - name: var_password_pam_maxsequence
      values:
        - '3'
---

# BER.6.3 - \[Passwortgebrauch\] Trivialpasswörter

## Control Statement

Berechtigung für Nutzende SOLLTE die Verwendung von Trivialpassworten blockieren.

## Control guidance

Trivialpasswörter sind leicht zu erratende oder zu diesem Zugangskonto bereits öffentlich bekannte Passwörter (erkennbar durch Nutzung sog. Leak Check Datenbanken). Leicht zu erraten sind Passwörter, wenn sie mit gängigen Wörterbuchangriffen (dictionary attacks) bzw. systematischem Ausprobieren (brute force) in kurzer Zeit zu kompromittieren sind. Dazu zählen etwa einfache Folgen wie „123456“, „Passwort“ oder „qwerty“ sowie häufig vorkommende, in Leaks dokumentierte Standardkombinationen. Der Zweck der Anforderung liegt darin, das Risiko unautorisierter Zugriffe zu reduzieren: Ein Angreifer könnte mit automatisierten Tools in Sekunden oder Minuten triviale Passwörter durchprobieren, was zu einem unbefugten Zugriff auf Benutzerkonten, Systemressourcen oder sensible Daten führen könnte. Die Blockierung solcher Passwörter kann dagegen sicherstellen, dass nur schwer vorhersehbare Kennwörter verwendet werden, wodurch ein entscheidender Schutz gegen automatisierte Angriffsverfahren erreicht werden kann. Zudem können Passwortmanager beim Generieren nicht-trivialer Passwörter unterstützen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Grundsätzlich sollte ein RHEL-Host an zentrale Identity-Provider/Verzeichnisdienste angebunden sein und an dieser Stelle die Passwort-Qualität durchgesetzt sein. Für lokale Accounts blockiert RHEL mit `pam_pwquality` die Trivial- und Wörterbuchpasswörter: über authselect-gesteuerte PAM-Zeilen oder alternativ  über `/etc/security/pwquality.conf` greifen `dictcheck`, Mindestlänge und Zeichenklassen; Passwortänderungen scheitern, wenn das neue Geheimnis Wörterbuchworten oder zu einfachen Mustern entspricht. Ein Abgleich mit öffentlichen Leak-Datenbanken ist kein PAM-Feature.

Weitere Informationen: [Authentifizierung und Autorisierung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_authentication_and_authorization_in_rhel/index).

### Rules:

  - package_pam_pwquality_installed
  - accounts_password_pam_pwquality_system_auth
  - accounts_password_pam_pwquality_password_auth
  - accounts_password_pam_dictcheck
  - accounts_password_pam_maxsequence
  - accounts_password_pam_maxrepeat

### Implementation Status: partial

______________________________________________________________________

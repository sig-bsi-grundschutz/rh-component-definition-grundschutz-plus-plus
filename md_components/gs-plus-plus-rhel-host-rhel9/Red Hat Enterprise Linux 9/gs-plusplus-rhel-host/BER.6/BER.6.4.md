---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: accounts_password_pam_dcredit
      description: accounts password pam dcredit
    - name: accounts_password_pam_dictcheck
      description: accounts password pam dictcheck
    - name: accounts_password_pam_lcredit
      description: accounts password pam lcredit
    - name: accounts_password_pam_minclass
      description: accounts password pam minclass
    - name: accounts_password_pam_minlen
      description: accounts password pam minlen
    - name: accounts_password_pam_ocredit
      description: accounts password pam ocredit
    - name: accounts_password_pam_pwquality_password_auth
      description: accounts password pam pwquality password auth
    - name: accounts_password_pam_pwquality_system_auth
      description: accounts password pam pwquality system auth
    - name: accounts_password_pam_ucredit
      description: accounts password pam ucredit
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_password_pam_dcredit
      description: var password pam dcredit
      options: 0,-1,-2
      rule-id: accounts_password_pam_dcredit
    - name: var_password_pam_dictcheck
      description: var password pam dictcheck
      options: '1'
      rule-id: accounts_password_pam_dictcheck
    - name: var_password_pam_lcredit
      description: var password pam lcredit
      options: 0,-1,-2
      rule-id: accounts_password_pam_lcredit
    - name: var_password_pam_minclass
      description: var password pam minclass
      options: 1,2,3,4
      rule-id: accounts_password_pam_minclass
    - name: var_password_pam_minlen
      description: var password pam minlen
      options: 10,12,14,15,17,18,20,6,7,8
      rule-id: accounts_password_pam_minlen
    - name: var_password_pam_ocredit
      description: var password pam ocredit
      options: 0,-1,-2
      rule-id: accounts_password_pam_ocredit
    - name: var_password_pam_ucredit
      description: var password pam ucredit
      options: 0,-1,-2
      rule-id: accounts_password_pam_ucredit
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
    - name: var_password_pam_dcredit
      values:
        - '-1'
    - name: var_password_pam_dictcheck
      values:
        - '1'
    - name: var_password_pam_lcredit
      values:
        - '-1'
    - name: var_password_pam_minclass
      values:
        - '3'
    - name: var_password_pam_minlen
      values:
        - '15'
    - name: var_password_pam_ocredit
      values:
        - '-1'
    - name: var_password_pam_ucredit
      values:
        - '-1'
---

# BER.6.4 - \[Passwortgebrauch\] Kriterien für die Qualität von Passwörtern

## Control Statement

Berechtigung SOLLTE Kriterien für die Qualität von Passwörtern anhand von Lebensdauer und Angriffsmöglichkeiten verankern.

## Control guidance

Kriterien für die Qualität von Passwörtern können z.B. eine minimale Entropie, Passwortlänge oder Verwendung verschiedener Symbole sein. Die Lebensdauer meint die erwartete Nutzungsdauer des Passwortes. Die erforderliche Qualität hängt von den Angriffsmöglichkeiten ab, z.B. Anzahl der Zugangskonten, verwendetes kryptografisches Verfahren (vgl. BSI TR-02102) und begleitenden Sicherheitsmaßnahmen wie maximale Passwortversuche oder Mehr-Faktor-Authentifizierung. Für Zugänge ohne begleitende Maßnahmen ist eine Passwortlänge nicht unter 14 Zeichen empfehlenswert. Die Kriterien können einmalig festgelegt werden oder zwischen Zugängen oder Anwendungen differenzieren.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Grundsätzlich sollte ein RHEL-Host an zentrale Identity-Provider/Verzeichnisdienste angebunden sein und an dieser Stelle die Passwort-Qualität durchgesetzt sein. Für lokale Accounts blockiert RHEL mit `pam_pwquality` die Trivial- und Wörterbuchpasswörter: über authselect-gesteuerte PAM-Zeilen oder alternativ  über `/etc/security/pwquality.conf` greifen `dictcheck`, Mindestlänge und Zeichenklassen; Passwortänderungen scheitern, wenn das neue Geheimnis Wörterbuchworten oder zu einfachen Mustern entspricht.

Weitere Informationen: [Authentifizierung und Autorisierung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_authentication_and_authorization_in_rhel/index).

### Rules:

  - accounts_password_pam_dictcheck
  - accounts_password_pam_minlen
  - accounts_password_pam_minclass
  - accounts_password_pam_dcredit
  - accounts_password_pam_ucredit
  - accounts_password_pam_lcredit
  - accounts_password_pam_ocredit
  - accounts_password_pam_pwquality_password_auth
  - accounts_password_pam_pwquality_system_auth

### Implementation Status: implemented

______________________________________________________________________

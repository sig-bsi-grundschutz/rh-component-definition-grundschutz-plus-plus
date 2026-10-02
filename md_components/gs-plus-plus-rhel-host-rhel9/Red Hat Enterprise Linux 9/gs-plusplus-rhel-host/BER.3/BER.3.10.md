---
x-trestle-param-values:
  ber.3.10-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: account_password_pam_faillock_password_auth
      description: account password pam faillock password auth
    - name: account_password_pam_faillock_system_auth
      description: account password pam faillock system auth
    - name: accounts_passwords_pam_faillock_deny
      description: accounts passwords pam faillock deny
    - name: accounts_passwords_pam_faillock_deny_root
      description: accounts passwords pam faillock deny root
    - name: accounts_passwords_pam_faillock_enabled
      description: accounts passwords pam faillock enabled
    - name: accounts_passwords_pam_faillock_even_deny_root_or_root_unlock_time
      description: accounts passwords pam faillock even deny root or root unlock
        time
    - name: accounts_passwords_pam_faillock_interval
      description: accounts passwords pam faillock interval
    - name: accounts_passwords_pam_faillock_unlock_time
      description: accounts passwords pam faillock unlock time
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_accounts_passwords_pam_faillock_deny
      description: var accounts passwords pam faillock deny
      options: 10,3,4,5,6,8
      rule-id: accounts_passwords_pam_faillock_deny
    - name: var_accounts_passwords_pam_faillock_root_unlock_time
      description: var accounts passwords pam faillock root unlock time
      options: 60,1800,3600,600,604800,86400,900,300,0
      rule-id: 
        accounts_passwords_pam_faillock_even_deny_root_or_root_unlock_time
    - name: var_accounts_passwords_pam_faillock_fail_interval
      description: var accounts passwords pam faillock fail interval
      options: 100000000,1800,3600,86400,900
      rule-id: accounts_passwords_pam_faillock_interval
    - name: var_accounts_passwords_pam_faillock_unlock_time
      description: var accounts passwords pam faillock unlock time
      options: 1800,3600,600,604800,86400,900,300,0
      rule-id: accounts_passwords_pam_faillock_unlock_time
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
    - name: var_accounts_passwords_pam_faillock_deny
      values:
        - '3'
    - name: var_accounts_passwords_pam_faillock_root_unlock_time
      values:
        - '0'
    - name: var_accounts_passwords_pam_faillock_fail_interval
      values:
        - '900'
    - name: var_accounts_passwords_pam_faillock_unlock_time
      values:
        - '0'
---

# BER.3.10 - \[Zugangskonten\] Anmeldeversuchsgrenze am System

## Control Statement

Berechtigung für IT-Systeme SOLLTE weitere Anmeldeversuche nach Erreichen von {{ insert: param, ber.3.10-prm1 }} fehlgeschlagenen Versuchen vorübergehend blockieren.

## Control guidance

Betrifft sowohl die lokale Anmeldung über eine Benutzeroberfläche als auch den Zugriff über Fernwartungsprotokolle oder -anwendungen wie RDP, SNMP, wenn diese vorhanden sind. Die Umsetzung erfolgt im einfachsten Fall durch ein Login, bzw. eine Bildschirmsperre für das IT-System. Biometrische Daten wie Fingerabdrücke können gefälscht werden und sind nicht so leicht zu ändern wie Passwörter. Setzen Sie Biometrie daher nicht als einzigen Authentifizierungsfaktor ein, sondern wenn, dann nur zur Ergänzung (Mehr-Faktor-Authentifizierung). Die Anforderung ist entbehrlich, wenn das System keinen Zugriff auf schützenswerte Daten erlaubt, z.B. bei Nutzung als Kiosk.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Red Hat Enterprise Linux implementiert die lokalen und via "Fernwartung" (=SSH)" Anmeldeversuchsgrenze über das PAM-Modul `pam_faillock`, das nach `authselect enable-feature with-faillock` in die PAM-Stacks `system-auth` und `password-auth` eingebunden wird und damit lokale Anmeldungen, `su`/`sudo` sowie SSH-Zugänge gleichermaßen erfasst. Die Parameter `deny` und `fail_interval` in `/etc/security/faillock.conf` legen den maximalen Schwellwert fehlgeschlagener Versuche und das Zeitfenster für deren Zählung fest, während `unlock_time` die vorübergehende Sperrdauer definiert; für das root-Konto lässt sich dies über `deny_root`/`root_unlock_time` gesondert steuern.

Weitere Informationen: [Benutzerauthentifizierung mit authselect konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_authentication_and_authorization_in_rhel/configuring-user-authentication-using-authselect_configuring-authentication-and-authorization-in-rhel)

### Rules:

  - accounts_passwords_pam_faillock_enabled
  - accounts_passwords_pam_faillock_deny
  - accounts_passwords_pam_faillock_unlock_time
  - accounts_passwords_pam_faillock_interval
  - account_password_pam_faillock_system_auth
  - account_password_pam_faillock_password_auth
  - accounts_passwords_pam_faillock_deny_root
  - accounts_passwords_pam_faillock_even_deny_root_or_root_unlock_time

### Implementation Status: implemented

______________________________________________________________________

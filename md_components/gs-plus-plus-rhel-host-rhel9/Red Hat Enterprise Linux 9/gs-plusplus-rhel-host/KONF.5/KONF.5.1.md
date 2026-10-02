---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: account_password_pam_modules_in_authselect_profile
      description: account password pam modules in authselect profile
    - name: account_password_pam_unix_enabled
      description: account password pam unix enabled
    - name: configure_crypto_policy
      description: configure crypto policy
    - name: dconf_gnome_screensaver_idle_delay
      description: dconf gnome screensaver idle delay
    - name: dconf_gnome_screensaver_lock_delay
      description: dconf gnome screensaver lock delay
    - name: dconf_gnome_screensaver_lock_enabled
      description: dconf gnome screensaver lock enabled
    - name: dconf_gnome_screensaver_user_locks
      description: dconf gnome screensaver user locks
    - name: enable_authselect
      description: enable authselect
    - name: gnome_gdm_disable_automatic_login
      description: gnome gdm disable automatic login
    - name: gnome_gdm_disable_unattended_automatic_login
      description: gnome gdm disable unattended automatic login
    - name: sshd_enable_pam
      description: sshd enable pam
    - name: sshd_include_crypto_policy
      description: sshd include crypto policy
    - name: sssd_enable_pam_services
      description: sssd enable pam services
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_system_crypto_policy
      description: var system crypto policy
      options: 
        DEFAULT,DEFAULT:NO-SHA1,FIPS,FIPS:OSPP,FIPS:STIG,LEGACY,FUTURE,NEXT
      rule-id: configure_crypto_policy
    - name: inactivity_timeout_value
      description: inactivity timeout value
      options: 600,900,1800,300
      rule-id: dconf_gnome_screensaver_idle_delay
    - name: var_screensaver_lock_delay
      description: var screensaver lock delay
      options: 10,5,0
      rule-id: dconf_gnome_screensaver_lock_delay
    - name: var_authselect_profile
      description: var authselect profile
      options: local,minimal,sssd
      rule-id: enable_authselect
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
    - name: var_system_crypto_policy
      values:
        - DEFAULT
    - name: inactivity_timeout_value
      values:
        - '900'
    - name: var_screensaver_lock_delay
      values:
        - '0'
    - name: var_authselect_profile
      values:
        - sssd
---

# KONF.5.1 - \[Authentifizierung\] Authentifizierung am System

## Control Statement

Konfiguration für IT-Systeme SOLLTE den Zugriff auf das System im Einklang mit den zugehörigen Anforderungen zum Identitäts- und Berechtigungsmanagement authentifizieren.

## Control guidance

Betrifft sowohl die lokale Anmeldung über eine Benutzeroberfläche als auch den Zugriff über Fernwartungsprotokolle oder -anwendungen wie RDP, SNMP, wenn diese vorhanden sind. Die Umsetzung erfolgt im einfachsten Fall durch einen Login, bzw. eine Bildschirmsperre für das IT-System. Biometrische Daten wie Fingerabdrücke können gefälscht werden und sind nicht so leicht zu ändern wie Passwörter. Setzen Sie Biometrie daher nicht als einzigen Authentifizierungsfaktor ein, sondern wenn, dann nur zur Ergänzung (Mehr-Faktor-Authentifizierung). Die Formulierung "im Einklang mit den zugehörigen Anforderungen zum Identitäts- und Berechtigungsmanagement" bedeutet, dass die Authentifizierung so erfolgt, wie in der Praktik Berechtigung (BER) festgelegt. Hierzu gehört insbesondere die Verwendung aktueller kryptographischer Verfahren, wie sie im Thema Kryptographie zu finden ist. Die Anforderung ist entbehrlich, wenn das System keinen Zugriff auf schützenswerte Daten erlaubt, z.B. bei Nutzung als Kiosk.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL erzwingt den Systemzugang über den PAM-Stack, den `authselect` mit getesteten Profilen (z. B. `sssd` oder `local`) konsistent konfiguriert. Bei Anbindung von PAM an die entsprechenden Tools (z.B. `sshd`) durchlaufen Anmeldungen damit durch dieselbe Authentifizierungskette. Zentrale Identitätsquellen (IdM, Active Directory) bindet SSSD Identitäten und Berechtigungen per NSS/PAM an. `sshd` berücksichtigt ebenfalls die crypto-policy des systems. Bei RHEL-Systemen mit GUI ist der automatische Login zu de- und Bildschirmschoner zu aktivieren.

Weitere Informationen: [Authentifizierung und Autorisierung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_authentication_and_authorization_in_rhel/index), [Benutzerauthentifizierung mit authselect konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_authentication_and_authorization_in_rhel/configuring-user-authentication-using-authselect_configuring-authentication-and-authorization-in-rhel)

### Rules:

  - enable_authselect
  - sshd_enable_pam
  - sssd_enable_pam_services
  - account_password_pam_modules_in_authselect_profile
  - account_password_pam_unix_enabled
  - gnome_gdm_disable_automatic_login
  - gnome_gdm_disable_unattended_automatic_login
  - dconf_gnome_screensaver_idle_delay
  - dconf_gnome_screensaver_lock_enabled
  - dconf_gnome_screensaver_lock_delay
  - dconf_gnome_screensaver_user_locks
  - configure_crypto_policy
  - sshd_include_crypto_policy

### Implementation Status: implemented

______________________________________________________________________

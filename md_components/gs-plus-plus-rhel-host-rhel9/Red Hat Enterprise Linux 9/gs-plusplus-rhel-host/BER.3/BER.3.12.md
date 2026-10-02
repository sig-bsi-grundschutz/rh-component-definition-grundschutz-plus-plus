---
x-trestle-param-values:
  ber.3.12-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: accounts_tmout
      description: accounts tmout
    - name: dconf_gnome_screensaver_idle_activation_enabled
      description: dconf gnome screensaver idle activation enabled
    - name: dconf_gnome_screensaver_idle_delay
      description: dconf gnome screensaver idle delay
    - name: dconf_gnome_screensaver_lock_delay
      description: dconf gnome screensaver lock delay
    - name: dconf_gnome_screensaver_lock_enabled
      description: dconf gnome screensaver lock enabled
    - name: dconf_gnome_session_idle_user_locks
      description: dconf gnome session idle user locks
    - name: logind_session_timeout
      description: logind session timeout
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_accounts_tmout
      description: var accounts tmout
      options: 1800,600,900,300
      rule-id: accounts_tmout
    - name: inactivity_timeout_value
      description: inactivity timeout value
      options: 600,900,1800,300
      rule-id: dconf_gnome_screensaver_idle_delay
    - name: var_screensaver_lock_delay
      description: var screensaver lock delay
      options: 10,5,0
      rule-id: dconf_gnome_screensaver_lock_delay
    - name: var_logind_session_timeout
      description: var logind session timeout
      options: 600,7200,840,900,1800,300,3600
      rule-id: logind_session_timeout
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
    - name: var_accounts_tmout
      values:
        - '600'
    - name: inactivity_timeout_value
      values:
        - '900'
    - name: var_screensaver_lock_delay
      values:
        - '0'
    - name: var_logind_session_timeout
      values:
        - '300'
---

# BER.3.12 - \[Zugangskonten\] Systemsperre bei Inaktivität

## Control Statement

Berechtigung für IT-Systeme SOLLTE eine Sperre bei Inaktivität nach {{ insert: param, ber.3.12-prm1 }} aktivieren.

## Control guidance

Kann durch eine Bildschirmsperre oder Abmeldung (Automatic Session Locking) umgesetzt werden. Eine längere Inaktivität kann z.B. 5-15 Minuten lang sein. Verwendet das System keine eigene Authentifizierung, so ist auch diese Anforderung entbehrlich.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL setzt eine automatische Sperre oder Abmeldung nach Inaktivität über mehrere, per OpenSCAP prüfbare Mechanismen um. Für grafische GNOME-Sitzungen (sofern bereitgestellt) aktivieren `idle-activation-enabled`, `idle-delay`, `lock-enabled` und `lock-delay` in `/etc/dconf/db/distro.d/00-security-settings` den Bildschirmschoner mit Sperre nach einer konfigurierbaren Frist (z.B. {{ insert: param, ber.3.12-prm1 }} ), gegen Nutzeränderung abgesichert durch entsprechende dconf-Locks. Für textbasierte Sitzungen (Konsole, SSH) erzwingt die schreibgeschützte Variable `TMOUT` in `/etc/profile.d/tmout.sh` eine automatische Abmeldung nach Inaktivität. Unabhängig von Desktop oder Shell beendet zusätzlich `systemd-logind` über `StopIdleSessionSec` in `/etc/systemd/logind.conf` jede als inaktiv erkannte Sitzung auf Betriebssystemebene.

Weitere Informationen: [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index)

### Rules:

  - dconf_gnome_screensaver_idle_activation_enabled
  - dconf_gnome_screensaver_idle_delay
  - dconf_gnome_screensaver_lock_enabled
  - dconf_gnome_screensaver_lock_delay
  - dconf_gnome_session_idle_user_locks
  - accounts_tmout
  - logind_session_timeout

### Implementation Status: implemented

______________________________________________________________________

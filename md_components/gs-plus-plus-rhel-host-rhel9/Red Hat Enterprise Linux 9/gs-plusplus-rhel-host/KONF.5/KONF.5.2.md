---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: accounts_max_concurrent_login_sessions
      description: accounts max concurrent login sessions
    - name: accounts_tmout
      description: accounts tmout
    - name: sshd_disable_root_login
      description: sshd disable root login
    - name: sshd_set_idle_timeout
      description: sshd set idle timeout
    - name: sshd_set_keepalive
      description: sshd set keepalive
    - name: sshd_set_max_sessions
      description: sshd set max sessions
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_accounts_max_concurrent_login_sessions
      description: var accounts max concurrent login sessions
      options: 1,10,15,20,3,5
      rule-id: accounts_max_concurrent_login_sessions
    - name: var_accounts_tmout
      description: var accounts tmout
      options: 1800,600,900,300
      rule-id: accounts_tmout
    - name: sshd_idle_timeout_value
      description: sshd idle timeout value
      options: 600,7200,840,900,1800,300,3600
      rule-id: sshd_set_idle_timeout
    - name: var_sshd_set_keepalive
      description: var sshd set keepalive
      options: 10,3,5,0,1
      rule-id: sshd_set_keepalive
    - name: var_sshd_max_sessions
      description: var sshd max sessions
      options: 10,4,3,2,1,0
      rule-id: sshd_set_max_sessions
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
    - name: var_accounts_max_concurrent_login_sessions
      values:
        - '1'
    - name: var_accounts_tmout
      values:
        - '600'
    - name: sshd_idle_timeout_value
      values:
        - '300'
    - name: var_sshd_set_keepalive
      values:
        - '0'
    - name: var_sshd_max_sessions
      values:
        - '10'
---

# KONF.5.2 - \[Authentifizierung\] Keine Mehrfachanmeldung

## Control Statement

Konfiguration für IT-Systeme SOLLTE die gleichzeitige Anmeldung mehrerer Zugangskonten deaktivieren.

## Control guidance

Wenn Nutzende mit verschiedenen Identitäten simultan im System angemeldet sind, erhöht sich das Risiko von versehentlichen Datenvermischungen oder Falscheingaben deutlich. Dies kann besonders in sensiblen Bereichen wie im Finanzwesen oder Gesundheitswesen schwerwiegende Folgen haben, wo vertrauliche Kundendaten oder Patienteninformationen unbeabsichtigt zwischen verschiedenen Kontexten übertragen werden könnten. Bei Vorfällen wird so auch erschwert herauszufinden, von welchem Zugangskonto bestimmte Ereignisse stammen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL kann über PAM und `pam_limits` in `/etc/security/limits.conf` bzw. `/etc/security/limits.d/` die Zahl gleichzeitiger interaktiver Anmeldungen **pro Zugangskonto** begrenzen (`maxlogins`); typisch sind Einträge wie `* hard maxlogins 10`, bei strenger Policy auch `1`. Damit wird pro Konto nur eine oder wenige parallele Sessions erlaubt. Für SSH kann zusätzlich `MaxSessions` in `sshd_config` parallele Kanäle pro Netzwerk-Verbindung drosseln, ohne verschiedene Identitäten zu verknüpfen. Um die gleichzeitige Anmeldung von verschiedenen Nutzerkonten zu verbieten, also effektiv nur eine gleichzeitige Anmeldung am System zu erlauben, muss `* maxsyslogins 1` in `/etc/security/limits.d/` gesetzt werden. `maxsyslogin` wirkt nicht auf den user `root` diesem kann aber explizit der Zugang via SSH untersagt werden. Es ist empfehlenswert bei einer solch strengen Konfiguration (`* maxsyslogins 1`) einen Notfall-Plan zu implementieren, wie man in dem Falle umgeht, dass eine Session dauerhaft offen ist und nicht geschlossen wird. Ebenfalls ist die Auswirkung auf potentielle Automatisierung zu berücksichtigen, die auf die Systeme zugreift. Session-Timeouts oder alternative Administrationswege (z.B. via Console) können hierbei unterstützend wirken.

Weitere Informationen: [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index), [Authentifizierung und Autorisierung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_authentication_and_authorization_in_rhel/index).

### Rules:

  - accounts_max_concurrent_login_sessions
  - sshd_set_max_sessions
  - sshd_disable_root_login
  - accounts_tmout
  - sshd_set_idle_timeout
  - sshd_set_keepalive

### Implementation Status: partial

______________________________________________________________________

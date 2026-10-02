---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: account_disable_post_pw_expiration
      description: account disable post pw expiration
    - name: account_temp_expire_date
      description: account temp expire date
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_account_disable_post_pw_expiration
      description: var account disable post pw expiration
      options: 0,180,30,35,40,45,60,90
      rule-id: account_disable_post_pw_expiration
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
    - name: var_account_disable_post_pw_expiration
      values:
        - '35'
---

# BER.2.5 - \[Identitätsmanagement\] Deaktivierung bei Weggang

## Control Statement

Berechtigung SOLLTE die zugeordnete Identität bei Weggang von Nutzenden deaktivieren.

## Control guidance

Weggang meint hier die nicht nur kurzfristige Beendigung der Aktivitäten der Identität, z.B. bei Kündigung, Elternzeit, Sabbatical. Die Anforderung ist auch umgesetzt, wenn die Identität gelöscht wird. Empfehlenswert ist die Löschung jedoch erst nach Ablauf längerer Löschfristen, um die Nachvollziehbarkeit von Aktionen im Audit Log zu erhalten.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL stellt die technischen Mittel bereit, um eine Identität sofort zu deaktivieren, ohne sie zu löschen: Mit `chage -E <Datum> <Konto>` wird ein Ablaufdatum gesetzt (z.B. das Tagesdatum beim Weggang), wonach das Konto keine Anmeldung mehr zulässt; alternativ sperrt `usermod -L`/`passwd -l` das Passwort sofort. Für befristete Konten erzwingt `account_temp_expire_date` ein Ablaufdatum bereits bei Einrichtung. Zusätzlich deaktiviert `account_disable_post_pw_expiration` Konten automatisch, wenn das Passwort abgelaufen ist und über einen konfigurierten Zeitraum nicht erneuert wurde. Über mehrere Hosts hinweg, können lokale personalisierte Konten mittels Ansible deaktiviert werden. In zentral über Red Hat IdM oder Active Directory verwalteten Umgebungen wirkt eine Deaktivierung im Verzeichnis über SSSD auf alle angebundenen RHEL-Hosts. Die eigentliche Auslösung — das Erkennen des Weggangs und das Anstoßen der Deaktivierung — bleibt ein organisatorischer HR-/Offboarding-Prozess außerhalb des Hosts.

### Rules:

  - account_temp_expire_date
  - account_disable_post_pw_expiration

### Implementation Status: partial

______________________________________________________________________

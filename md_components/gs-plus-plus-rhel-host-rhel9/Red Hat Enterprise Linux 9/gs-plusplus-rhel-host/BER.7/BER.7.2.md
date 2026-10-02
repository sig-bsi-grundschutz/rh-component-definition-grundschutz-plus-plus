---
x-trestle-param-values:
  ber.7.2-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: configure_crypto_policy
      description: configure crypto policy
    - name: crypto_policy_not_legacy
      description: crypto policy not legacy
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_system_crypto_policy
      description: var system crypto policy
      options: 
        DEFAULT,DEFAULT:NO-SHA1,FIPS,FIPS:OSPP,FIPS:STIG,LEGACY,FUTURE,NEXT
      rule-id: configure_crypto_policy
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
---

# BER.7.2 - \[Schlüsselmanagement\] Schlüssellänge

## Control Statement

Berechtigung SOLLTE die Schlüssellängen nach {{ insert: param, ber.7.2-prm1 }} bei der Schlüsselerzeugung zuweisen.

## Control guidance

Für die Sicherheit von Schlüsseln wie Passwörter oder PINs ist die Länge von Bedeutung. Für Details siehe BSI TR-02102.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Mindestschlüssellängen bei der Erzeugung asymmetrischer Schlüssel erzwingt RHEL über die systemweite Crypto Policy: Im Profil `DEFAULT` sind RSA- und Diffie-Hellman-Parameter unter 2048 Bit sowie ECC unter 256 Bit unzulässig; `FUTURE` erhöht die Grenzen weiter. OpenSSH, OpenSSL, GnuTLS und weitere Backends übernehmen diese Grenzen bei `ssh-keygen`, Zertifikatserstellung und TLS-Handshake automatisch. Passwort- und PIN-Längen für menschliche Geheimnisse regelt hingegen PAM/`pwquality` (BER.6) — nicht die Crypto Policy.

Weitere Informationen: [Systemweite kryptografische Richtlinien](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/using-the-system-wide-cryptographic-policies_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Rules:

  - configure_crypto_policy
  - crypto_policy_not_legacy

### Implementation Status: implemented

______________________________________________________________________

---
x-trestle-param-values:
  ber.7.1-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: configure_crypto_policy
      description: configure crypto policy
    - name: configure_gnutls_tls_crypto_policy
      description: configure gnutls tls crypto policy
    - name: configure_kerberos_crypto_policy
      description: configure kerberos crypto policy
    - name: configure_libreswan_crypto_policy
      description: configure libreswan crypto policy
    - name: configure_openssl_crypto_policy
      description: configure openssl crypto policy
    - name: configure_openssl_tls_crypto_policy
      description: configure openssl tls crypto policy
    - name: configure_ssh_crypto_policy
      description: configure ssh crypto policy
    - name: crypto_policy_not_legacy
      description: crypto policy not legacy
    - name: crypto_policy_not_overridden
      description: crypto policy not overridden
    - name: package_crypto-policies_installed
      description: package crypto-policies installed
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

# BER.7.1 - \[Schlüsselmanagement\] Etablierte Algorithmen bei der Schlüsselerzeugung

## Control Statement

Berechtigung SOLLTE die ausschließliche Verwendung etablierter kryptografischer Algorithmen bei der Schlüsselerzeugung nach {{ insert: param, ber.7.1-prm1 }} verankern.

## Control guidance

Etablierte kryptografische Algorithmen sind mathematisch fundierte Verschlüsselungsverfahren und Protokolle, die in der aktuellen Praxis nicht mit vertretbarem Aufwand gebrochen werden können. Sie basieren auf mathematisch schwer lösbaren Problemen, bieten Resistenz gegen bekannte kryptanalytische Angriffe, unterstützen ausreichend große Schlüssellängen und wurden von Experten gründlich geprüft und analysiert. Aktuelle etablierte Algorithmen sind in BSI TR-02102 zu finden. Für weitere Details zur Implementierung siehe Detailspezifikation kryptografischer Abläufe und Mechanismen des BSI.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Bei der Schlüsselerzeugung und -nutzung erzwingt RHEL etablierte Algorithmen und ausreichende Parameter über systemweite Crypto Policies (`update-crypto-policies`): Stufen wie `DEFAULT`, `FUTURE` oder `FIPS` steuern zulässige Verfahren in OpenSSL, GnuTLS, NSS, Libreswan und weiteren Backends und schließen veraltete Algorithmen bzw. zu kurze Schlüssel aus. Neu erzeugte Schlüsselmaterialien und TLS-/IPsec-/SSH-Aushandlungen erben damit dieselbe Policy; Abweichungen über dienstspezifische Overrides sollten vermieden werden (`crypto_policy_not_overridden`). Eigene Policy-Profile (z. B. an BSI TR-02102 angelehnt) sind möglich, erfordern aber Fachwissen und Tests, damit Schlüssellängen und Verfahren nicht unbeabsichtigt abgeschwächt werden.

Weitere Informationen: [Systemweite kryptografische Richtlinien](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/using-the-system-wide-cryptographic-policies_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Rules:

  - configure_crypto_policy
  - configure_gnutls_tls_crypto_policy
  - configure_kerberos_crypto_policy
  - configure_libreswan_crypto_policy
  - configure_openssl_crypto_policy
  - configure_openssl_tls_crypto_policy
  - configure_ssh_crypto_policy
  - crypto_policy_not_legacy
  - crypto_policy_not_overridden
  - package_crypto-policies_installed

### Implementation Status: implemented

______________________________________________________________________

---
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
    - name: package_crypto_policies_installed
      description: package crypto policies installed
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

# BER.7.8 - \[Schlüsselmanagement\] Etablierte Algorithmen bei der Schlüsselnutzung

## Control Statement

Berechtigung SOLLTE die ausschließliche Verwendung etablierter Algorithmen bei der Schlüsselnutzung verankern.

## Control guidance

Aktuelle etablierte Algorithmen sind in BSI TR-02102 zu finden. Für weitere Details zur Implementierung siehe Detailspezifikation kryptografischer Abläufe und Mechanismen des BSI.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Bei der operativen Schlüsselnutzung (TLS-Handshakes, SSH-Sitzungen, Kerberos, IPsec) wendet RHEL die systemweite Crypto Policy an: Alle policy-fähigen Bibliotheken und Dienste verwenden dieselben erlaubten Algorithmen und Schlüssellängen, sodass bei Verschlüsselung, Signatur und Authentisierung nur etablierte Verfahren zum Einsatz kommen. OpenSSH bindet die Policy über Crypto-Policy-Drop-ins ein; TLS-Dienste nutzen OpenSSL/GnuTLS-Backends.

Weitere Informationen: [Systemweite kryptografische Richtlinien](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_using-the-system-wide-cryptographic-policies_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

Rules:

- configure_crypto_policy
- configure_gnutls_tls_crypto_policy
- configure_kerberos_crypto_policy
- configure_libreswan_crypto_policy
- configure_openssl_crypto_policy
- configure_openssl_tls_crypto_policy
- configure_ssh_crypto_policy
- crypto_policy_not_legacy
- crypto_policy_not_overridden
- package_crypto_policies_installed

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
  - package_crypto_policies_installed

### Implementation Status: implemented

______________________________________________________________________

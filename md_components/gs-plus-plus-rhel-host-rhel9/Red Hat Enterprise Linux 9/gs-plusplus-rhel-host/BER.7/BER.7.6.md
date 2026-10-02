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

# BER.7.6 - \[Schlüsselmanagement\] Etablierte Algorithmen beim Transport

## Control Statement

Berechtigung SOLLTE die ausschließliche Verwendung etablierter kryptografischer Algorithmen beim Transport geheimer Schlüssel verankern.

## Control guidance

Aktuelle etablierte Algorithmen sind in BSI TR-02102 zu finden. Der Transport kann mit Public Key Cryptography Standards (PKCS), z.B. PKCS#12 Dateiformat erfolgen. Für weitere Details zur Implementierung siehe Detailspezifikation kryptografischer Abläufe und Mechanismen des BSI.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Beim Transport geheimer Schlüssel über das Netz (TLS, SSH, IPsec) erzwingt RHEL etablierte Algorithmen über die systemweite Crypto Policy: TLS-Versionen, Cipher Suites, KEX- und MAC-Algorithmen für OpenSSL/GnuTLS/NSS sowie OpenSSH werden zentral gesteuert und schwache Verfahren deaktiviert. PKCS#12-Exporte und verschlüsselte Übertragungen profitieren von denselben OpenSSL-Policy-Backends.

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
  - package_crypto_policies_installed

### Implementation Status: implemented

______________________________________________________________________

---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: configure_crypto_policy
      description: configure crypto policy
    - name: configure_ssh_crypto_policy
      description: configure ssh crypto policy
    - name: sshd_include_crypto_policy
      description: sshd include crypto policy
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
x-trestle-param-values:
  konf.14.1-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.14.1 - \[Verteilte Anwendungen\] Verschlüsselung beim Transport

## Control Statement

Konfiguration für Anwendungen SOLLTE Kommunikation beim Transport über Netze nach {{ insert: param, konf.14.1-prm1 }} verschlüsseln.

## Control guidance

Werden Daten unverschlüsselt übertragen, so könnten sie abgehört oder unbemerkt manipuliert werden. Relevant sind hierbei alle von der Anwendung übertragenen Daten, inklusive Authentifizierung an der Benutzerschnittstelle oder API, Abruf von Daten, Server-Server-Replikation oder zur Datensicherung. Das betrifft sowohl Inhalts- als auch Metadaten. Die Umsetzung kann mit Algorithmen zur Transportverschlüsselung wie Transport Layer Security (TLS) oder Ende-zu-Ende-Verschlüsselung erfolgen. Für aktuelle Verschlüsselungsverfahren siehe BSI TR-02102. Die Konfiguration der Verschlüsselung kann sich daran orientieren, wie lange die transportieren Daten, z.B. Transaktionen, vertraulich zu behandeln sind. Eine Herausforderung hierbei sind Anwendungen, die über allgemeine Anbindungen mit anderen Institutionen kommunizieren, z.B. E-Mails oder Anrufe ins öffentliche Telefonnetz. Diese Anwendungen können nur ihren Teil der Verbindungsstrecke verschlüsseln, so dass der Rest der Strecke und damit die Verbindung an sich dennoch unverschlüsselt sein könnte. Überträgt die Anwendung keine schützenswerten Daten über das Netz, so ist die Anforderung entbehrlich.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Auf Host-Ebene erzwingt RHEL 9 über die systemweite Crypto Policy (`update-crypto-policies --set <Profil>`) einheitliche Mindestanforderungen für TLS, DTLS und verwandte Bibliotheken (OpenSSL, GnuTLS, NSS, libkrb5): Im Profil DEFAULT sind unter anderem TLS-Versionen unter 1.2, kurze RSA-/DH-Schlüssel und veraltete Chiffren deaktiviert; das Profil FUTURE verschärft die Vorgaben weiter (z. B. TLS 1.3). OpenSSH-Server und -Client beziehen Cipher, MACs und Key-Exchange über dieselbe Policy, sofern `sshd` die Crypto-Policy-Konfiguration einbindet und nicht durch lokale Overrides aus der Systemrichtlinie ausbricht. Damit sind Transportverbindungen der plattformnahen Dienste (z. B. administrative SSH-Zugriffe, TLS-fähige Systemkomponenten) kryptographisch abgesichert, sofern die Institution ein zum Schutzbedarf passendes Policy-Profil wählt (Parameter `konf.14.1-prm1`, abgestimmt mit BSI TR-02102). Anwendungsspezifische Protokolle (Web-Apps, Datenbankreplikation, E-Mail-Relay) konfiguriert die Institution in den jeweiligen Diensten; RHEL stellt dafür keine zentrale „Anwendungs-TLS“-Schaltstelle bereit.

Weitere Informationen: [Systemweite kryptografische Richtlinien](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/using-the-system-wide-cryptographic-policies_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index)

### Implementation Status: partial

______________________________________________________________________

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
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.14.1.1 - \[Verteilte Anwendungen\] Obligatorische Verschlüsselung

## Control Statement

Konfiguration für Anwendungen SOLLTE unverschlüsselte und anfällige Verbindungen über Netze deaktivieren.

## Control guidance

Obligatorische Verschlüsselung bedeutet, dass die Anwendung ausschließlich nach dem Stand der Technik verschlüsselt kommuniziert. Unverschlüsselte oder mit bekannten Methoden angreifbare Verbindungsanfragen werden dagegen abgelehnt. Die Verwendung obligatorischer Verschlüsselung im Internet ist aktuell sehr uneinheitlich: Viele E-Mail-Server z.B. verschlüsseln im Auslieferungszustand nur opportunistisch - also nur wenn der Verbindungsaufbau so funktioniert. Das macht Verbindungen anfällig für Downgrade-Angriffe. Diese lassen sich verhindern, indem unverschlüsselte Verbindungen vollständig deaktiviert werden. Andererseits kann es dadurch auch zu Verbindungsproblemen mit Servern kommen, die überhaupt keine Verschlüsselung mit aktuellen Protokollen unterstützen. Für aktuelle Verschlüsselungsverfahren siehe BSI TR-02102.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

<!-- Add control implementation description here for control: KONF.14.1.1 -->

Obligatorische Verschlüsselung auf dem Host bedeutet, dass Dienste keine bekannt schwachen oder unverschlüsselten Protokollvarianten mehr anbieten sollen. RHEL setzt dies für policy-fähige TLS-/DTLS-Stacks über die systemweite Crypto Policy um: Im Profil DEFAULT lehnt der System-TLS-Stack bereits Verbindungen mit TLS < 1.2 und viele veraltete Algorithmen ab; das Profil FUTURE geht weiter und erlaubt praktisch nur noch TLS 1.3 und moderne Parameter — geeignet, wenn Downgrade- oder „opportunistische“ Klartext-Fallbacks vermieden werden sollen. OpenSSH nutzt dieselbe Policy für Cipher und Key Exchange, sofern der Server die Crypto-Policy-Include-Datei lädt und keine abweichenden `Ciphers`/`MACs` in `sshd_config` erzwingt. Für obligatorische Verschlüsselung in Anwendungen (z. B. nur TLS, kein HTTP-Klartext) sind Dienstkonfiguration, Reverse-Proxies und Firewall-Regeln organisatorisch festzulegen; RHEL erzwingt das nicht pro Anwendungsinstanz.

Weitere Informationen: [Systemweite kryptografische Richtlinien](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/using-the-system-wide-cryptographic-policies_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index)

### Implementation Status: partial

______________________________________________________________________

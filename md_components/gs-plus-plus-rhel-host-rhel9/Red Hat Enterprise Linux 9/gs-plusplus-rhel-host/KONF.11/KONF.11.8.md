---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: encrypt_partitions
      description: encrypt partitions
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.11.8 - \[Vertrauensbeziehungen\] Verschlüsselung schützenswerter Daten (at-rest)

## Control Statement

Konfiguration für Anwendungen KANN schützenswerte Daten bei der Speicherung (at-rest) verschlüsseln.

## Control guidance

Hierbei ist insbesondere an Zugangsdaten zu denken. Die Anforderung ist auch dann erfüllt, wenn Daten statt einer Verschlüsselung mit Hash und Salt versehen sind. Zur Umsetzung siehe BSI TR-02102.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Die Anforderung betrifft Anwendungskonfigurationen (KANN): RHEL erzwingt nicht, dass jede installierte Anwendung schützenswerte Daten bei der Speicherung verschlüsselt; Institutionen setzen das in den jeweiligen Anwendungen um oder erfüllen die Guidance durch Hashing mit Salt (z. B. für Zugangsdaten) gemäß BSI TR-02102. Auf Host-Ebene kann RHEL ergänzend LUKS-Vollverschlüsselung mit `cryptsetup`/dm-crypt bereitstellen (Anaconda/Kickstart `--encrypted`, nachträgliche `crypto_LUKS`-Partitionen), wodurch Daten auf dem Datenträger auch bei Verlust oder Diebstahl des Systems erschwert auslesbar sind — das ist kein Ersatz für anwendungsspezifischen Schutz, sondern eine ergänzende Maßnahme für Daten, die auf dem Host persistiert werden.

Weitere Informationen: [Blockgeräte mit LUKS verschlüsseln](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/encrypting-block-devices-using-luks_security-hardening)

### Implementation Status: partial

______________________________________________________________________

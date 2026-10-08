---
x-trestle-param-values:
  ber.7.7-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_ber_7_7
      description: Narrative implementation seed for BER.7.7 (no CaC rule 
        binding)
---

# BER.7.7 - \[Schlüsselmanagement\] Kein Transport privater Schlüssel

## Control Statement

Berechtigung KANN den Export privater Schlüssel durch {{ insert: param, ber.7.7-prm1 }} autorisieren.

## Control guidance

Im Allgemeinen ist es sinnvoll, private Schlüssel nur dort zu erzeugen, wo sie auch genutzt werden. Andernfalls könnten sie durch den Export kompromittiert werden. Hiervon sind allerdings zahlreiche Ausnahmen denkbar, z.B. zur Schlüsselerzeugung auf besonders abgesicherten Systemen, zum Transport auf Redundanzsysteme oder zur Datensicherung. Daher ist eine Abwägung sinnvoll, ob der Export zu genehmigen ist.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL erzwingt keinen generellen Verbot des Exports oder Transports privater Schlüsseldateien: Schlüssel können lokal mit OpenSSH oder OpenSSL erzeugt und als Dateien kopiert, archiviert oder auf andere Systeme übertragen werden. Entsprechend der Anforderung obliegt die Genehmigung solcher Exporte der Institution (zuständige Person oder Rolle) in Betriebskonzept, Schlüsselmanagement und Prozessen — der Host prüft diese Freigabe nicht automatisch. Technisch unterstützt RHEL das Prinzip „Schlüssel dort erzeugen, wo sie genutzt werden“, indem Werkzeuge zur lokalen Erzeugung und Speicherung unter kontrollierten Pfaden (z. B. Benutzer-`~/.ssh`) bereitstehen und Dateirechte sowie SELinux den Zugriff einschränken. Wo private Schlüssel nicht als exportierbare Dateien vorkommen sollen, können Smartcards oder Hardware-Security-Module über PKCS #11 genutzt werden; der Schlüssel bleibt dann auf dem Token, während nur der öffentliche Teil verteilt wird. Verschlüsselte Backups oder Redundanzszenarien bleiben organisatorisch abzuwägen und sind vom Betriebssystem nicht pauschal verboten oder freigegeben.

Weitere Informationen: [Authentifizierung mit SSH-Schlüsseln auf einer Smartcard](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_authenticating-by-ssh-keys-stored-on-a-smart-card_security-hardening), [Kryptografische Hardware und PKCS #11](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_using-hardware-security-modules_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Implementation Status: partial

______________________________________________________________________

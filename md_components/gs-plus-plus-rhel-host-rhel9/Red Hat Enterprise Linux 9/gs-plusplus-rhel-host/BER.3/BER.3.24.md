---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# BER.3.24 - \[Zugangskonten\] Alternative Authentifizierung am IT-System

## Control Statement

Berechtigung für IT-Systeme KANN ein ebenso vertrauenswürdiges, alternatives Verfahren zur Authentifizierung verankern.

## Control guidance

Wenn Nutzende ein für die reguläre Authentisierung erforderliches Authentisierungsmittel verlieren oder nicht mehr verwenden können, kann eine alternative Möglichkeit zur Wiederherstellung des Zugangs relevant sein. Hierfür kommen sowohl bereits eingerichtete alternative Authentisierungsmittel als auch besondere Wiederherstellungsverfahren in Betracht. Ein vergleichbares Vertrauensniveau der alternativen Authentisierung oder Wiederherstellung trägt dazu bei, dass der Schutz der regulären Authentisierung nicht unangemessen herabgesetzt wird. Als alternative Authentisierungsmittel kommen beispielsweise ein zusätzlich registrierter kryptografischer Authenticator, eine Smartcard oder andere geeignete Authentisierungsverfahren in Betracht. Möglichkeiten zur Wiederherstellung sind beispielsweise sicher hinterlegte Recovery-Codes bzw. PUKs, die Verifikation über ein noch verfügbares Authentisierungsmittel oder eine bestehende vertrauenswürdige Sitzung sowie eine erneute Identitätsprüfung, etwa durch persönliche Vorstellung und Prüfung eines amtlichen Ausweisdokuments.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL kann über SSSD und `authselect` ein alternatives, PKI-gestütztes Authentifizierungsverfahren mittels Smartcard oder Zertifikat bereitstellen, das eine mit dem primären Anmeldeverfahren vergleichbare Vertrauenswürdigkeit erreicht: `authselect select sssd with-smartcard` aktiviert die Zertifikatsauthentifizierung zusätzlich zum Passwort, während `pam_cert_auth = True` in `/etc/sssd/sssd.conf` PAM anweist, die Karte zu prüfen. Über eine hinterlegte Vertrauensanker-CA, Sperrlisten-/OCSP-Prüfung (`certificate_verification`) sowie Zertifikatszuordnungsregeln (`sssd_certmap.conf`) wird sichergestellt, dass nur gültige, eindeutig einem Konto zugeordnete Zertifikate zur Anmeldung akzeptiert werden. Damit deckt RHEL die technische Kernanforderung eines gleichwertigen Alternativverfahrens ab; die in der Anleitung beispielhaft genannten Wiederherstellungswege bei Verlust des Primärzugangs — persönliche Vorstellung mit Ausweis, Verifikation über eine bestehende Sitzung oder PUK-Rücksetzung der Smartcard selbst — bleiben organisatorische bzw. kartenverwaltungsseitige Prozesse außerhalb der SSSD/PAM-Konfiguration des Hosts.

Weitere Informationen: [Smartcard-Authentifizierung mit authselect konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_smart_card_authentication/configuring-smart-cards-using-authselect_managing-smart-card-authentication)

### Implementation Status: partial

______________________________________________________________________

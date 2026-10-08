---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: ensure_gpgcheck_globally_activated
      description: ensure gpgcheck globally activated
    - name: ensure_gpgcheck_local_packages
      description: ensure gpgcheck local packages
    - name: ensure_gpgcheck_never_disabled
      description: ensure gpgcheck never disabled
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.5.10.1 - \[Management von Schwachstellen\] Autorisierte Bezugsquellen

## Control Statement

Detektion SOLLTE zuverlässige Bezugsquellen für Patches autorisieren.

## Control guidance

Eine Quelle ist unzuverlässig, wenn zukünftig mit Verstößen gegen die Schutzziele Vertraulichkeit, Verfügbarkeit oder Integrität durch die Entität zu rechnen ist (d.h. eine Prognose der Vertrauenswürdigkeit). Dies ist insbesondere der Fall, wenn erhebliche Verstöße gegen die Schutzziele durch die Entität begangen worden sind oder Anzeichen dafür vorliegen, dass bei einer Verwendung mit solchen Verstößen zu rechnen ist.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Für System- und Sicherheitsupdates bezieht RHEL Patches über die mit Subscription Manager verwalteten Repository-Definitionen (typisch `/etc/yum.repos.d/redhat.repo`) von der Red Hat Content Delivery Network oder von einem von der Institution autorisierten internen Spiegel (z. B. Red Hat Satellite). Vor der Installation prüft DNF/RPM die GPG-Signaturen der RPM-Pakete, wenn `gpgcheck=1` global in `/etc/dnf/dnf.conf` gesetzt ist, in keinem aktivierten Repository deaktiviert wird und für lokale Installationen die Paketsignaturprüfung aktiv bleibt. Zusätzliche Repositories und deren Schlüssel müssen organisatorisch freigegeben und in DNF konfiguriert werden; die Prognose der Vertrauenswürdigkeit einer Quelle selbst erfüllt das Betriebssystem nicht.

Weitere Informationen: [Software mit dem DNF-Tool verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/index), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Implementation Status: partial

______________________________________________________________________

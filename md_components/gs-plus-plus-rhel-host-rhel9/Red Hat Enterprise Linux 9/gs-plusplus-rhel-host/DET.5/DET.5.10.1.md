---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_5_10_1
      description: Narrative implementation seed for DET.5.10.1 (no CaC rule
        binding)
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

Red Hat Software wird prinzipiell ausschließlich von Red Hat über das von Red Hat betriebene Red Hat Content Delivery Network bereitgestellt. Der Bereitstellungsprozess wird hierbei nach SLSA Level 3 durchgeführt. Für Red Hat Software sollte Red Hat daher grundsätzlich eine vertrauenswürdige Quelle darstellen. Für System- und Sicherheitsupdates bezieht RHEL Patches über die mit Subscription Manager verwalteten Repository-Definitionen (typisch `/etc/yum.repos.d/redhat.repo`) von der Red Hat Content Delivery Network oder von einem von der Institution autorisierten internen Spiegel (z. B. Red Hat Satellite). Für auf RHEL aufsetzende Software können zusätzliche Repositories und deren Schlüssel organisatorisch freigegeben und in DNF konfiguriert werden.

Weitere Informationen: [Software mit dem DNF-Tool verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/index), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index), [SLSA Level 3](https://slsa.dev), [Red Hat Supply Chain Security Assurance](https://access.redhat.com/articles/software%20_supply_chain_security_assurance)

### Implementation Status: partial

______________________________________________________________________

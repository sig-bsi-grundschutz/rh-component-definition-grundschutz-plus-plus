---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: ensure_gpgcheck_globally_activated
      description: ensure gpgcheck globally activated
    - name: ensure_gpgcheck_local_packages
      description: ensure gpgcheck local packages
    - name: ensure_gpgcheck_never_disabled
      description: ensure gpgcheck never disabled
    - name: ensure_redhat_gpgkey_installed
      description: ensure redhat gpgkey installed
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.5.10.4 - \[Management von Schwachstellen\] Integritätsprüfung von Patches

## Control Statement

Detektion SOLLTE Patches vor der Installation auf Integrität testen.

## Control guidance

Wenn Patches durch Fehler bei der Übertragung oder sogar bewusst von Angreifern verändert wurden, kann dies nach der Installation zu nicht behebbaren Fehlerzuständen oder zur Verbreitung von Schadcode führen. Kann durch einen Abgleich von Prüfsummen umgesetzt werden, z.B. durch automatisierte Installationsroutinen oder einen manuellen Abgleich mit der Herstellerwebseite.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL prüft die Integrität von Patches vor der Installation kryptographisch über DNF/RPM: Pakete aus Repositories und lokale RPMs werden nur installiert, wenn ihre GPG-Signatur zum importierten Red-Hat-Schlüssel (bzw. den für freigegebene Repos konfigurierten Schlüsseln) passt. In der globalen DNF-Konfiguration und je Repository bleibt `gpgcheck` aktiviert; ein Abschalten pro Repo ist nicht vorgesehen. Damit entspricht die automatisierte Installationsroutine dem geforderten Prüfsummen-/Signaturabgleich vor dem Einspielen. Manuelle Abgleiche mit Herstellerseiten für Nicht-RPM-Artefakte bleiben organisatorisch zu regeln, sofern solche Quellen genutzt werden.

Weitere Informationen: [Software mit dem DNF-Tool verwalten](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/index), [Software management (Considerations in adopting RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/considerations_in_adopting_rhel_9/assembly_software-management_considerations-in-adopting-rhel-9), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index)

### Rules:

  - ensure_redhat_gpgkey_installed
  - ensure_gpgcheck_globally_activated
  - ensure_gpgcheck_local_packages
  - ensure_gpgcheck_never_disabled

### Implementation Status: implemented

______________________________________________________________________

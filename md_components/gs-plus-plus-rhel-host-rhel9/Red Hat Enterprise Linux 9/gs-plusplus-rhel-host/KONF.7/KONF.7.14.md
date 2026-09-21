---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.7.14 - \[Schutz vor Schadcode\] Code-Signierung im Betriebssystemkern

## Control Statement

Konfiguration für IT-Systeme SOLLTE die Signaturprüfung für nachladbaren Code im Kernelmodus aktivieren.

## Control guidance

Nachladbarer Code im Kernelmodus verfügt typischerweise über weitreichende Berechtigungen und kann bei einer Kompromittierung erhebliche Auswirkungen auf das gesamte IT-System haben. Eine Signaturprüfung kann dazu beitragen, das Laden von nicht oder nicht vertrauenswürdig signiertem Code im Kernelmodus zu verhindern. Beispiele sind die erzwungene Signaturprüfung von Kernelmodulen unter Linux oder die Signaturprüfung von Kernelmodus-Treibern unter Windows. Die Schutzwirkung der Signaturprüfung hängt von den verwendeten Vertrauensankern ab. Hierbei kann insbesondere berücksichtigt werden, welche Signaturschlüssel beziehungsweise Herausgeber als vertrauenswürdig eingestuft werden. Eine gültige Signatur allein erlaubt keine Aussage über die Sicherheit oder Qualität des signierten Codes.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL kann den Einsatz signierter Kernel-Module erzwingen — unsignierte Module werden beim Laden abgewiesen (`modprobe`/`kernel` module signing). Kernel-Images und kmod-Pakete stammen im Standard aus signierten Red-Hat-Builds. Dies addressiert das Control Statement ("Code-Signierung im Betriebssystemkern"). Für das Ausführen von vertrauenswürdigen Skripten und Anwendungen (Langtext aus der Control Guidance) sind die Maßnahmen der vorherigen KONF.7.x zu berücksichtigen.

Weitere Informationen: [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index), [Signing a Kernel](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_monitoring_and_updating_the_kernel/signing-a-kernel-and-modules-for-secure-boot_assembly_managing-kernel-command-line-parameters-with-uki)

### Rules:

  - kernel_config_module_sig
  - kernel_config_module_sig_all

### Implementation Status: implemented

______________________________________________________________________

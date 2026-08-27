---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.2.9 - \[Konfiguration von Systemen\] Abgesicherter und authentisierter Bootprozess

## Control Statement

Konfiguration für IT-Systeme KANN einen abgesicherten und authentisierten Bootprozess aktivieren.

## Control guidance

Dies empfiehlt sich für eingebettete Systeme (Embedded Systems), indem z.B. der Bootloader die Integrität des Betriebssystems überprüft und es nur dann lädt, wenn es als korrekt eingestuft wurde. Ebenso empfiehlt es sich ein mehrstufiges Boot-Konzept mit kryptographisch sicherer Überprüfung der Einzelschritte zu realisieren, sichere Hardware-Vertrauensanker zu verwenden, bei ARM & UEFI-basierten Systemem jeweils (ARM) Secure Boot zu nutzen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Auf UEFI-Systemen unterstützt RHEL einen mehrstufig authentisierten Bootprozess: Der Bootloader `shim` sowie GRUB2 und der Kernel sind mit Red-Hat-Schlüsseln signiert und werden von der UEFI-Firmware anhand der Microsoft-UEFI-CA- bzw. Red-Hat-Schlüssel in der `db`-Zertifikatsdatenbank verifiziert, bevor sie ausgeführt werden. Für zusätzliche, selbst signierte Kernel oder Fremd-Kernelmodule (z. B. DKMS-Treiber) steht die Machine-Owner-Key-Infrastruktur (`mokutil`) bereit, mit der ein eigenes Schlüsselpaar in die Firmware-Vertrauensliste eingebracht wird, ohne die UEFI-`db` direkt zu verändern. Der aktuelle Status lässt sich mit `mokutil --sb-state` prüfen.

### Rules:

  - secure_boot_enabled

### Implementation Status: planned

______________________________________________________________________

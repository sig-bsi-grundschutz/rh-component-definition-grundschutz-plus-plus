---
x-trestle-param-values:
  konf.6.1.2-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.6.1.2 - \[Rollen und Berechtigungen\] Isolierung von Anwendungen

## Control Statement

Konfiguration für IT-Systeme KANN die Isolierung von {{ insert: param, konf.6.1.2-prm1 }} aktivieren.

## Control guidance

Die Isolation von Anwendungen (auch Application Sandboxing oder Application Confinement genannt) kann die Auswirkungen von Fehlfunktionen oder einer Kompromittierung auf andere Anwendungen und Systemressourcen begrenzen. Hierzu werden Zugriffe einer Anwendung auf beispielsweise Prozesse, Dateisystembereiche, Netzwerkressourcen oder Geräte auf die für ihren Betrieb vorgesehenen Ressourcen und Schnittstellen beschränkt. „Bestimmte Anwendungen“ bezeichnet die Anwendungen, für die aufgrund ihres Einsatzzwecks oder der damit verbundenen Risiken eine isolierte Ausführung vorgesehen ist. Die Isolation kann mit unterschiedlichen technischen Mechanismen umgesetzt werden. Hierzu zählen beispielsweise Betriebssystemfunktionen zur Zugriffsbeschränkung wie SELinux- oder AppArmor-Profile, containerbasierte Isolation oder die Ausführung in virtuellen Maschinen. Die verschiedenen Verfahren bieten unterschiedliche Isolationsstärken. Container teilen sich typischerweise den Kernel des Hostsystems, während virtuelle Maschinen zusätzlich über ein eigenes Gastbetriebssystem und einen eigenen Kernel gegenüber dem Hostsystem abgegrenzt sind.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Die Isolation von Anwendungen ermöglicht RHEL primär über SELinux im enforcing-Modus, der im Standard aktiviert ist und bereits ab Boot aktiv ist: Prozesse laufen in getrennten Domains mit minimalen Rechten auf Dateien, Capabilities und Netzwerk-Ports. Datei-POSIX-Rechte, Capability-Binding und systemd-Unit-Hardening (`ProtectSystem`, `PrivateTmp`) reduzieren zusätzlich unnötige Privilegien. Die notwendigen Berechtigungen/Anwendungsprofile werden bei Software, die aus Red Hat Repositories stammt typischerweise mitgeliefert und installiert. Zusätzlich ist es möglich Anwendungen auf RHEL containerisiert mittels `podman` auszuführen und sie so zusätzlich zu kapseln. Es ist ebenfalls möglich auf einem RHEL Host mittels `kvm` und `qemu` entsprechende virtuelle Maschinen zu erzeugen und Workloads so zu kapseln. Dies ist jedoch gerade für größere Umgebungen keine Lösung, die auch Hochverfügbarkeitsaspekte berücksichtigt. In solchen Fällen sollte auf Red Hat OpenShift-Virtualization oder 3rd Party Virtualisierungslösungen zurückgegriffen werden. Die feste Zuordnung von Containern sollte maximal zu einer Container-Host-Gruppe erfolgen um Verfügbarkeitsziele zu erreichen.

Weitere Informationen: [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Rules:

  - selinux_state
  - selinux_policytype
  - selinux_not_disabled
  - grub2_enable_selinux
  - selinux_confinement_of_daemons

### Implementation Status: implemented

______________________________________________________________________

---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: dnf-automatic_apply_updates
      description: configure dnf-automatic to install available updates automatically
    - name: dnf-automatic_security_updates_only
      description: configure dnf-automatic to install only security updates
    - name: package_dnf-automatic_installed
      description: install dnf-automatic package
    - name: timer_dnf-automatic_enabled
      description: enable dnf-automatic timer
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.8.1.1 - \[Sicherheitsupdates\] Automatische Sicherheitsupdates

## Control Statement

Konfiguration für IT-Systeme SOLLTE Sicherheitsupdates automatisch installieren.

## Control guidance

Dies kann durch direkten Download vom Hersteller oder einen eigenen Verteilerserver umgesetzt werden, so lange dieser ebenfalls automatisch aktuell gehalten wird. Damit Sicherheitsupdates des Betriebssystems auch tatsächlich wirken und Fehlerzustände vermieden werden ist typischerweise ein Neustart erforderlich, damit die Betriebssystemfunktionen und damit verbundene Anwendungen aus dem installierten Update neu geladen und in einen definierten Zustand versetzt werden. Manche Systeme unterstützen alternativ auch Live-Patching des Betriebssystems im laufenden Betrieb.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

<!-- Add control implementation description here for control: KONF.8.1.1 -->

Auf RHEL 9 installiert das Paket `dnf-automatic` einen Dienst, der verfügbare Updates regelmäßig per DNF prüft und installieren kann. In `/etc/dnf/automatic.conf` wird im Abschnitt `[commands]` `upgrade_type = security` gesetzt, damit nur Sicherheitsupdates berücksichtigt werden, und `apply_updates = yes`, damit diese Updates automatisch eingespielt werden. Der systemd-Timer `dnf-automatic-install.timer` lädt die Konfiguration periodisch und führt die Installation aus; dafür muss eine gültige Red-Hat-Subscription am Host hinterlegt sein. Nach Kernel- oder Bibliotheksupdates können Neustarts oder Dienste-Neuladen nötig bleiben, damit alle Komponenten den neuen Stand nutzen — das betrifft Betriebsprozesse außerhalb der reinen Paketinstallation.

Weitere Informationen: [Installing security updates automatically (RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_and_monitoring_security_updates/installing-security-updates_managing-and-monitoring-security-updates)

### Implementation Status: partial

______________________________________________________________________

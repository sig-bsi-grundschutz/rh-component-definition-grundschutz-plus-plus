---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_not_4_2
      description: Narrative implementation seed for NOT.4.2 (no CaC rule 
        binding)
x-trestle-param-values:
  not.4.2-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# NOT.4.2 - \[Datensicherung\] Sicherung des Systems

## Control Statement

Notfallplanung für IT-Systeme SOLLTE deren Datensicherung {{ insert: param, not.4.2-prm1 }} ausführen.

## Control guidance

Zu den erforderlichen Daten können z.B. Konfigurationsdateien des Betriebssystems, Firmware, Lizenzen, Treiber und die Systemdokumentation gehören. Bei gleichartigen Systemen kann die Anforderung auch durch die Sicherung einer Kopie erfolgen, wenn mit dieser alle IT-Systeme dieser Art funktionsfähig wiederhergestellt werden können. Die Anforderung kann auch durch die Wiederherstellung aus einem Versionskontrollsystem erfolgen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Für die Sicherung und Wiederherstellung eines RHEL Hosts gibt es mehrere Möglichkeiten. Die klassische Variante ist die Nutzung von REAR (RElax-And-Recover), einem mit RHEL ausgelieferten Backup-Tool, das ein einzelnes Betriebssystem entsprechend der ReaR-Konfiguration sichert. Eine Alternative in diesem Ansatz ist die Verwendung von 3rd Party-Backup-Lösungen, die entweder auf Ebene der Speicherlösung, der Virtualisierungslösung oder des Betriebssystems ausgeführt werden. Eine moderne Alternative bietet der "Infrastructure-as-code" Ansatz, in dem die Systemkonfiguration inklusive aller Konfigurationsdaten in einem Versionskontrollsystem (Git) mittels Ansible-Ressourcen definiert ist und so gleichartige Systeme jederzeit "from Scratch" wiederhergestellt werden können. Dies kann - je nach Implementierung in den Ansible-Playbooks - inklusive der Anwendungen und der dazugehörige Konfiguration erfolgen. Ein Ausgangspunkt hierfür sind die mit RHEL ausgelieferten `system-roles`. Ein dritter Ansatz ist die Nutzung des Image-Modes von RHEL. In diesem Modus wird das Betriebssystem inklusive Konfiguration und zu installierender Anwendungen in einem `Containerfile` definiert und anschließend ausgerollt. Hierdurch entsteht mit jeder Veränderung des Betriebssystems - sei es Konfigurationsänderung oder Update - ein neues System-Image, dass identisch zu Container-Images verteilt und verwaltet wird. Die Notwendigkeit der Sicherung einzelner Systeme entfällt dabei, da diese jederzeit vom Image wiederherstellbar sind. Anwendungsbackups sind in dieser Betrachtung nicht berücksichtigt.

Weitere Informationen: [Sichern und Wiederherstellen mit ReaR](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/assembly_recovering-and-restoring-a-system_configuring-basic-system-settings), [RHEL System Roles mit Ansible (Infrastructure-as-Code)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/automating_system_administration_by_using_rhel_system_roles/index), [Image Mode for RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/using_image_mode_for_rhel_to_build_deploy_and_manage_operating_systems/introducing-image-mode-for-rhel_using-image-mode-for-rhel-to-build-deploy-and-manage-operating-systems)

### Implementation Status: partial

______________________________________________________________________

---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: fapolicy_default_deny
      description: fapolicy default deny
    - name: grub2_enable_selinux
      description: grub2 enable selinux
    - name: package_fapolicyd_installed
      description: package fapolicyd installed
    - name: package_libselinux_installed
      description: package libselinux installed
    - name: selinux_confinement_of_daemons
      description: selinux confinement of daemons
    - name: selinux_not_disabled
      description: selinux not disabled
    - name: selinux_policytype
      description: selinux policytype
    - name: selinux_state
      description: selinux state
    - name: service_fapolicyd_enabled
      description: service fapolicyd enabled
x-trestle-rules-params:
  Red Hat Enterprise Linux 9:
    - name: var_selinux_policy_name
      description: var selinux policy name
      options: targeted,mls
      rule-id: selinux_policytype
    - name: var_selinux_state
      description: var selinux state
      options: enforcing,disabled,permissive
      rule-id: selinux_state
x-trestle-comp-def-rules-param-vals:
  # You may set new values for rule parameters by adding
  #
  # component-values:
  #   - value 1
  #   - value 2
  #
  # below a section of values:
  # The values list refers to the values as set by the components, and the component-values are the new values
  # to be placed in SetParameters of the component definition.
  #
  Red Hat Enterprise Linux 9:
    - name: var_selinux_policy_name
      values:
        - targeted
    - name: var_selinux_state
      values:
        - enforcing
---

# KONF.7.10 - \[Schutz vor Schadcode\] Einschränkung der Ausführung

## Control Statement

Konfiguration für IT-Systeme SOLLTE die Ausführung nicht autorisierter Anwendungen einschränken.

## Control guidance

Wenn Anwendungen an beliebigen Speicherorten installiert und ausgeführt werden, z.B. im Wurzeldateisystem des Betriebssystems oder an Speicherorten zusammen mit Daten der Nutzerumgebung, dann könnte dies zahlreiche Sicherheitsrisiken mit sich bringen. Unbefugte oder schadhafte Anwendungen könnten unbemerkt an unautorisierte Orte platziert werden, wo sie außerhalb etablierter Sicherheitskontrollen agieren und beispielsweise Privilege-Escalation-Angriffe durchführen können. Zudem wird das Risiko der Manipulation von Anwendungsdateien erhöht, da Angreifer gezielt nach nicht-geschützten Speicherorten suchen, um dort eigenen Code zu hinterlegen oder legitime Anwendungen zu modifizieren. Eine solche Situation kann zu "Living-off-the-Land"-Angriffen führen, bei denen Angreifer vorhandene legitime Programme missbrauchen, um Schadaktionen auszuführen, was die Erkennung erheblich erschwert. Die Beschränkung von Ausführungsspeicherorten (Execution Control) zielt darauf ab, die Angriffsfläche zu reduzieren und eine bessere Kontrolle über ausführbare Programme zu ermöglichen. Zur technischen Umsetzung dieser Anforderung kann eine Institution verschiedene Maßnahmen implementieren. Application Allowlisting kann eingesetzt werden, um nur vertrauenswürdige Anwendungen aus definierten Verzeichnissen auszuführen, beispielsweise mittels AppLocker unter Windows oder SELinux unter Linux-Systemen. Zusätzlich können Software Restriction Policies (SRPs) konfiguriert werden, um Ausführungsrechte auf bestimmte Verzeichnispfade zu begrenzen, wobei eine Trennung zwischen Systemverzeichnissen und Nutzerverzeichnissen empfehlenswert ist. Weitere wirksame Techniken umfassen die Implementierung von Code Signing, wodurch nur digital signierte Anwendungen ausgeführt werden können, sowie die Nutzung von Container-Technologien wie Docker, die eine isolierte Ausführungsumgebung bieten. Dabei kann auf das Inventar der Anwendungen als Grundlage zurückgegriffen werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

SELinux beschränkt im Auslieferungszustand die Ausführung von Programmen und ihre Berechtigungen auf dem System, verhindert allerdings kein "Living of the Land" Szenario. Die Ausführung nicht autorisierter Programme wird durch `fapolicyd` mit deny-all/permit-by-exception-Policy eingeschränkt. Datenpartitionen können zusätzlich mittles `noexec` eingeschränkt werden. Die Ausführung von Containern kann via `podman` geschehen.

Weitere Informationen: [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).

### Rules:

  - fapolicy_default_deny
  - package_fapolicyd_installed
  - service_fapolicyd_enabled
  - package_libselinux_installed
  - grub2_enable_selinux
  - selinux_not_disabled
  - selinux_policytype
  - selinux_state
  - selinux_confinement_of_daemons

### Implementation Status: partial

______________________________________________________________________

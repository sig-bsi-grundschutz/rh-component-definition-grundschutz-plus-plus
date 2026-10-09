---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_det_4_1
      description: Narrative implementation seed for DET.4.1 (no CaC rule
        binding)
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# DET.4.1 - \[Überwachung von Aktivitäten\] Überwachung der Protokollierung

## Control Statement

Detektion SOLLTE die Funktionsfähigkeit der Protokollierung überwachen.

## Control guidance

Zu den Kriterien kann beispielsweise die Aktivierung oder Deaktkvierung des Loggings auf Systemen, sowie die Datenmenge eingehender Logs in einem bestimmten Zeitraum gehören.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

<!-- Add control implementation description here for control: DET.4.1 -->

 RHEL stellt für die Überwachung der RHEL-seitigen Logging-Komponenten Bausteine bereit: `systemd` verwaltet die Dienste `auditd`, `rsyslog` bzw. `systemd-journald`, deren Zustand lässt sich mit `systemctl` und über Journal-Einträge prüfen. Dies kann als Indikator für eine externe Monitoring-Lösung verwendet werden, in der die Log-Funktionalität überwacht wird. Dies trifft jedoch bei der Anbindung eines zentralen Log-Managements (SIEM) keine Aussage darüber, ob die Daten auch dort anschauen. Dies sollte in der ganzheitlichen Betrachtung berücksichtigt werden. Ein einzelner Host liefert keine fertige Erkennung „Logging deaktiviert“ oder „Logvolumen außerhalb Toleranz“ ohne zusätzliche Automatisierung oder zentrale Korrelation.

Weitere Informationen: [Logging-RHEL-Systemrolle](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_using-the-logging-system-role_security-hardening), [Audit-Aufzeichnungen konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_configuring-audit-records_security-hardening)

### Implementation Status: partial

______________________________________________________________________

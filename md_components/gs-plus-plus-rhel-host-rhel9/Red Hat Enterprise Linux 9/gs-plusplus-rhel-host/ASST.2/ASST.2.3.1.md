---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: fapolicy_default_deny
      description: fapolicy default deny
    - name: package_fapolicyd_installed
      description: package fapolicyd installed
    - name: service_fapolicyd_enabled
      description: service fapolicyd enabled
x-trestle-param-values:
  asst.2.3.1-prm1:
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# ASST.2.3.1 - \[Inventarisierung\] Autorisierung von Anwendungen

## Control Statement

Informationen und Assets für IT-Systeme SOLLTE die Nutzung von Anwendungen auf diesen durch {{ insert: param, asst.2.3.1-prm1 }} autorisieren.

## Control guidance

Der Sinn dieser Regelung liegt in der Minimierung von Risiken, die durch unkontrollierte Nutzung entstehen. Durch nicht autorisierte Anwendungen könnte beispielsweise Schadsoftware in die Systeme der Institution eingeschleust, könnten durch Sicherheitslücken in veralteter Software Angriffsvektoren geöffnet oder könnten durch den Einsatz nicht konformer Tools sensible Informationen unkontrolliert abfließen. Ein strukturierter Autorisierungsprozess kann somit die Integrität der IT-Systeme wahren und sicherstellen, dass nur geprüfte, für den Geschäftszweck erforderliche und aus rechtlicher Sicht unbedenkliche Anwendungen zum Einsatz kommen, was die gesamte Angriffsfläche der Institution signifikant reduziert. Relevant sind dabei sowohl lokal installierte Anwendungen, als auch solche, die auf Cloud-Servern oder in verteilten Diensten betrieben werden. Je nach Geschäftsprozessen oder Risikoprofil kann die Autorisierung einzeln für jedes System und jede Anwendung, oder für bestimmte Kategorien von Systemen oder Anwendungen vorgenommen werden (z.B. "Alle Office-Produkte eines bestimmten Herstellers auf Notebooks mit einem bestimmten Betriebssystem). Hierbei ist es sinnvoll, Standard-Anwendungen zu bestimmen, die für alle Nutzenden freigegeben sind und die Verwendung darüber hinausgehender Anwendungen pro Nutzer oder Organisationseinheit zu autorisieren.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

<!-- Add control implementation description here for control: ASST.2.3.1 -->

Auf RHEL kann die Ausführung von Anwendungen technisch an eine Freigabeliste gebunden werden: `fapolicyd` wertet Prozess- und Dateiattribute aus und setzt im Regelwerk eine Deny-by-default- bzw. Permit-by-exception-Policy um, sodass nur explizit erlaubte Programme starten. Die Installation weiterer Software über `dnf` erfordert in der Regel erhöhte Rechte. Signierte Pakete aus vertrauenswürdigen Repositories ergänzen die technische Kette. Die in der Anforderung genannte Autorisierung durch eine zuständige Person oder Rolle (Freigabeprozess, Kategorien, Inventar, Cloud- und Standardanwendungen) bleibt organisatorisch zu definieren und in die `fapolicyd`-Regeln bzw. Softwarequellen (z. B. kuratierte Repositories über Red Hat Satellite) zu übersetzen. Der Host erzwingt allein nicht den fachlichen Freigabeworkflow.

Weitere Informationen: [Anwendungen mit fapolicyd blockieren und zulassen](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/assembly_blocking-and-allowing-applications-using-fapolicyd_security-hardening), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index)

### Implementation Status: partial

______________________________________________________________________

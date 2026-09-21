---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# KONF.4.1.1 - \[Vertrauenswürdige Basisdienste\] Weiterleitung von Anmeldeinformationen

## Control Statement

Konfiguration für IT-Systeme SOLLTE die Weiterleitung mehrfach verwendbarer Anmeldeinformationen deaktivieren.

## Control guidance

„Weiterleitung mehrfach verwendbarer Anmeldeinformationen“ (auch als Credential Forwarding oder Credential Delegation bezeichnet) meint Mechanismen, bei denen Anmeldeinformationen oder daraus abgeleitete Authentisierungsinformationen an ein weiteres System übertragen oder diesem zur weiteren Authentisierung zur Verfügung gestellt werden. Dadurch können auf dem Zielsystem Informationen oder Authentisierungsfähigkeiten verfügbar werden, die bei einer Kompromittierung für weitere Zugriffe missbraucht werden könnten. Die Einschränkung der Weiterleitung kann das Risiko reduzieren, dass Angreifende nach der Kompromittierung eines Systems Anmeldeinformationen oder daraus abgeleitete Authentisierungsinformationen für laterale Bewegungen verwenden. Insbesondere bei privilegierten Zugangskonten kann dadurch vermieden werden, dass wiederverwendbare Anmeldeinformationen auf weniger vertrauenswürdigen Systemen verfügbar werden. Für Remotezugriffe können Verfahren eingesetzt werden, bei denen die Anmeldeinformationen nicht an das Zielsystem übertragen werden. Beispiele unter Windows sind Remote Credential Guard oder Restricted Admin für Remotedesktopverbindungen. Bei SSH-Verbindungen kann auf die Weiterleitung des lokalen SSH-Authentisierungsagenten verzichtet werden. Wird Agent Forwarding nicht benötigt, kann dessen Verwendung client- und serverseitig eingeschränkt werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Auf RHEL wird die Weiterleitung wiederverwendbarer Anmeldeinformationen über OpenSSH primär serverseitig in `/etc/ssh/sshd_config` bzw. Drop-in-Dateien unter `/etc/ssh/sshd_config.d/` unterbunden: `DisableForwarding yes` deaktiviert alle Forwarding-Funktionen einschließlich SSH-Agent-Forwarding, TCP- und X11-Weiterleitung; alternativ lassen sich `AllowAgentForwarding no` und `AllowTcpForwarding no` gezielt setzen. Für Kerberos/GSSAPI-Delegation kann ergänzend `GSSAPIDelegateCredentials no` gesetzt werden. GSSAPI-Ticket-Weiterleitung außerhalb von SSH, Pass-the-Hash aus dem Speicher kompromittierter Dienste oder clientseitig erzwungenes Agent-Forwarding auf Servern mit lockerer sshd-Policy bleiben organisatorisch bzw. im Gesamtdesign zu adressieren.

Weitere Informationen: [Netzwerke absichern](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/htmlsingle/securing_networks/index), [Sichere Kommunikation mit OpenSSH](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/assembly_using-secure-communications-between-two-systems-with-openssh_configuring-basic-system-settings).

### Rules:

  - sshd_disable_forwarding
  - sshd_disable_x11_forwarding
  - sshd_disable_tcp_forwarding

### Implementation Status: implemented

______________________________________________________________________

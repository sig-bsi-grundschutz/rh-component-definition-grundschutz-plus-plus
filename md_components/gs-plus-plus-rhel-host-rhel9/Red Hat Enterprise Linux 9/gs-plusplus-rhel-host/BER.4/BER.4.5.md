---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_ber_4_5
      description: Narrative implementation seed for BER.4.5 (no CaC rule 
        binding)
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# BER.4.5 - \[Berechtigungsmanagement\] Systemfunktionen ohne Authentifizierung

## Control Statement

Berechtigung für IT-Systeme SOLLTE Funktionen, auf die ohne vorherige Authentifizierung zugegriffen werden kann, dokumentieren.

## Control guidance

Funktionen ohne Authentifizierung sind alle Zugriffsmöglichkeiten auf Schnittstellen oder Daten des Systems, für die keine Authentifizierung erforderlich ist. Hierzu gehören z. B. Sprachassistenten, offene Webserver-Ports oder das Einblenden von Inhalten aus Apps auf dem Sperrbildschirm, wodurch persönliche Nachrichten oder Logintoken für Unbefugte zugänglich sein könnten. Solche Funktionen sind häufige Einfallstore für Angriffe auf das System oder für Datenleaks. Daher ist es sinnvoll eine Übersicht dieser Funktionen zu führen, selbst wenn die Funktionen benötigt werden.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Die Anforderung verlangt die Dokumentation von Systemfunktionen, die ohne Authentifizierung erreichbar sind. Auf dem RHEL-Host unterstützen `firewalld` (Zonen, freigegebene Services/Ports) und Abfragen der lauschenden Sockets (`ss -tulpn`, `firewall-cmd --list-all`) die Inventarisierung netzseitig exponierter Einstiegspunkte ohne Login. Authentifizierungsfreie lokale Interfaces (z. B. Cockpit nur nach Login, anonyme NFS-/HTTP-Freigaben, Sperrbildschirm-Inhalte auf Clients) sind je nach installierten Diensten zu erfassen. Die geforderte Übersicht selbst ist ein Pflegeprozess der Institution; der Host liefert die technischen Faktenquellen, erzwingt aber kein Dokumentationsartefakt.

Weitere Informationen: [Firewalls und Paketfilter konfigurieren](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_firewalls_and_packet_filters/index), [Sicherheitshärtung](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/index).


### Implementation Status: alternative

______________________________________________________________________

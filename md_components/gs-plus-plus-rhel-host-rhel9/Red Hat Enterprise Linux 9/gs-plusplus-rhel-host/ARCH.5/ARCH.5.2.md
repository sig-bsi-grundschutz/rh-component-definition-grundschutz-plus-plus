---
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: gspp_impl_arch_5_2
      description: Narrative implementation seed for ARCH.5.2 (no CaC rule 
        binding)
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
---

# ARCH.5.2 - \[Perimeterschutz\] Blockieren direkter öffentlicher Verbindungen

## Control Statement

Architektur für IT-Systeme SOLLTE direkte Verbindungen von diesen ins öffentliche Netz blockieren.

## Control guidance

Direkte Verbindungen sind hier alle Verbindungen, die nicht von der Filterung erfasst werden. Die Anforderung ist für Firewallsysteme umgesetzt, wenn deren eingehende Verbindungen ebenfalls vollständig gefiltert werden, bevor sie Daten an Systemschnittstellen senden können.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

<!-- Add control implementation description here for control: ARCH.5.2 -->

Die Anforderung verlangt, dass IT-Systeme keine ungefilterten Verbindungen ins öffentliche Netz aufbauen; auf einem RHEL‑9‑Host unterstützt **firewalld** Zonen, Richtlinien und Dienste, um ein- und ausgehenden sowie weitergeleiteten Verkehr gezielt zu erlauben oder zu verweigern (typisch: restriktive Zone wie `public` oder `drop`, nur explizit freigegebene Dienste). Wo firewalld nicht ausreicht, lassen sich mit **nftables** Ausgangs‑Ketten (`output`) und organisationsspezifische Regeln umsetzen, die direkten Internetzugang einschränken und Verkehr über kontrollierte Pfade (Proxy, Perimeter‑Firewall, geschütztes Transitnetz) erzwingen. RHEL liefert dafür keine feste Vorgabe „kein direkter Internetzugang“; Routing, Adressierung und die Entscheidung, ob der Host selbst als Firewall fungiert, bleiben Teil der Netzarchitektur und Betriebsprozesse der Institution. Weitere Informationen: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_firewalls_and_packet_filters/using-and-configuring-firewalld_firewall-packet-filters, https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_firewalls_and_packet_filters/getting-started-with-nftables_firewall-packet-filters

### Implementation Status: partial

______________________________________________________________________

---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: mask_nonessential_services
      description: mask nonessential services
    - name: service_abrtd_disabled
      description: service abrtd disabled
    - name: service_atd_disabled
      description: service atd disabled
    - name: service_autofs_disabled
      description: service autofs disabled
    - name: service_avahi-daemon_disabled
      description: service avahi-daemon disabled
    - name: service_bluetooth_disabled
      description: service bluetooth disabled
    - name: service_cups_disabled
      description: service cups disabled
    - name: service_debug-shell_disabled
      description: service debug-shell disabled
    - name: service_dhcpd_disabled
      description: service dhcpd disabled
    - name: service_dnsmasq_disabled
      description: service dnsmasq disabled
    - name: service_dovecot_disabled
      description: service dovecot disabled
    - name: service_httpd_disabled
      description: service httpd disabled
    - name: service_kdump_disabled
      description: service kdump disabled
    - name: service_named_disabled
      description: service named disabled
    - name: service_nfs_disabled
      description: service nfs disabled
    - name: service_ntpdate_disabled
      description: service ntpdate disabled
    - name: service_oddjobd_disabled
      description: service oddjobd disabled
    - name: service_qpidd_disabled
      description: service qpidd disabled
    - name: service_rdisc_disabled
      description: service rdisc disabled
    - name: service_rexec_disabled
      description: service rexec disabled
    - name: service_rhnsd_disabled
      description: service rhnsd disabled
    - name: service_rlogin_disabled
      description: service rlogin disabled
    - name: service_rpcbind_disabled
      description: service rpcbind disabled
    - name: service_rsyncd_disabled
      description: service rsyncd disabled
    - name: service_slapd_disabled
      description: service slapd disabled
    - name: service_smb_disabled
      description: service smb disabled
    - name: service_snmpd_disabled
      description: service snmpd disabled
    - name: service_squid_disabled
      description: service squid disabled
    - name: service_telnet_disabled
      description: service telnet disabled
    - name: service_vsftpd_disabled
      description: service vsftpd disabled
    - name: service_xinetd_disabled
      description: service xinetd disabled
    - name: service_ypserv_disabled
      description: service ypserv disabled
---

# KONF.2.4 - \[Konfiguration von Systemen\] Deaktivierung nicht benötigter Systemfunktionen

## Control Statement

Konfiguration für IT-Systeme SOLLTE nicht benötigte Systemfunktionen deaktivieren.

## Control guidance

Die Deaktivierung von Funktionen, die für Betrieb oder aus Sicherheitssicht nicht benötigt werden, hilft, die Angriffsfläche und Fehlerkomplexität zu verringern, z.B. unnötige Identitäten, ggf. nicht benötigte Schnittstellen wie Bluetooth, nicht verwendete Netzprotokolle wie NTLMv1 Authentifizierung, schwache Verschlüsselungsalgorithmen wie TLS1.1, die Anzeige von Nachrichteninhalten auf dem Sperrbildschirm oder nicht benötigte System- oder Telemetriedienste. Relevant sind dabei sowohl Betriebssystem- als auch Firmwarefunktionen.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

RHEL erlaubt das gezielte Deaktivieren nicht benötigter Systemfunktionen auf mehreren Ebenen: Netzwerkdienste werden über systemd dauerhaft gestoppt und maskiert (`systemctl mask --now <dienst>.service`, ggf. inklusive zugehöriger `.socket`-Unit), sodass sie weder manuell noch als Abhängigkeit eines anderen Dienstes erneut starten können. Nicht benötigte Kernel-Module (Dateisysteme, veraltete Netzprotokolle) werden über modprobe-Blacklist-/`install /bin/false`-Konfiguration in `/etc/modprobe.d/` am Laden gehindert. Welche konkreten Dienste, Schnittstellen (z. B. Bluetooth) und Protokolle als "nicht benötigt" gelten, hängt vom Einsatzzweck des Systems ab und ist eine Entscheidung der Institution.

### Rules:

  - service_avahi-daemon_disabled
  - service_abrtd_disabled
  - service_kdump_disabled
  - service_ntpdate_disabled
  - service_oddjobd_disabled
  - service_qpidd_disabled
  - service_rdisc_disabled
  - service_rhnsd_disabled
  - service_atd_disabled
  - service_dhcpd_disabled
  - service_named_disabled
  - service_dnsmasq_disabled
  - service_vsftpd_disabled
  - service_httpd_disabled
  - service_dovecot_disabled
  - service_slapd_disabled
  - service_rpcbind_disabled
  - service_nfs_disabled
  - service_xinetd_disabled
  - service_ypserv_disabled
  - service_rexec_disabled
  - service_rlogin_disabled
  - service_rsyncd_disabled
  - service_telnet_disabled
  - service_cups_disabled
  - service_squid_disabled
  - service_smb_disabled
  - service_snmpd_disabled
  - service_debug-shell_disabled
  - service_bluetooth_disabled
  - service_autofs_disabled
  - mask_nonessential_services

### Implementation Status: partial

______________________________________________________________________

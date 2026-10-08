---
x-trestle-global:
  profile:
    title: Grundschutz++ für Red Hat Enterprise Linux Host
    href: trestle://profiles/gs-plusplus-rhel-host/profile.json
x-trestle-comp-def-rules:
  Red Hat Enterprise Linux 9:
    - name: grub2_uefi_password
      description: The grub2 boot loader should have a superuser account and 
        password protection enabled to protect boot-time settings.
    - name: grub2_uefi_admin_username
      description: The grub2 boot loader should have a superuser account and 
        password protection enabled to protect boot-time settings.
---

# KONF.5.1.1 - \[Authentifizierung\] Authentifizierung an der Firmware

## Control Statement

Konfiguration für IT-Systeme SOLLTE den Zugriff auf die Firmware im Einklang mit den zugehörigen Anforderungen zum Identitäts- und Berechtigungsmanagement authentifizieren.

## Control guidance

Durch unautorisierte Änderungen an Einstellungen der Firmware (UEFI oder Embedded System) könnten Fehlerzustände entstehen oder Sicherheitsfunktionen wie TPM deaktiviert werden. Dies kann je nach Firmware durch lokale Zugangspasswörter oder zentrale Berechtigung umgesetzt werden. Hierbei sind insbesondere Einstellungen von Sicherheitsfunktionen oder der Netzanbindung relevant. Die Formulierung "im Einklang mit den zugehörigen Anforderungen zum Identitäts- und Berechtigungsmanagement" bedeutet, dass die Authentifizierung so erfolgt, wie in der Praktik Berechtigung (BER) festgelegt. Hierzu gehört insbesondere die Verwendung aktueller kryptographischer Verfahren, wie sie im Thema Kryptographie zu finden ist.

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

Auf UEFI-Systemen schützt RHEL den nächsten Boot-Pfad über GRUB 2: Mit `grub2-setpassword` wird ein Superuser-Konto und ein PBKDF2-Hash in `/boot/grub2/user.cfg` hinterlegt; in `/etc/grub.d/01_users` legt die Institution einen eindeutigen Superuser-Namen (nicht `root`, `admin` oder bestehende Konten) fest und aktualisiert die Konfiguration mit `grub2-mkconfig`. Beim Bearbeiten von Boot-Einträgen (z. B. Taste `e` im GRUB-Menü) sind Benutzername und Passwort erforderlich, sodass Kernel-Parameter oder Wartungsmodi nicht ohne Authentifizierung geändert werden können. Das adressiert den Übergang von Firmware/UEFI zum Betriebssystem, ersetzt aber nicht das Setup-Passwort der Plattform-Firmware: Zugriff auf UEFI-Setup, Secure Boot, TPM oder Netzwerk-Optionen der Firmware wird herstellerabhängig außerhalb von RHEL (BIOS/UEFI-Setup, KVM-Konsolenrichtlinien, physischer Zugangsschutz) abgesichert und ist nicht an zentrales IAM anbindbar. Passwortvergabe, Rotation und Freigabe für GRUB- und Firmware-Zugänge bleiben organisatorische Aufgaben gemäß BER.

Weitere Informationen: [GRUB-Bootloader konfigurieren (RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_monitoring_and_updating_the_kernel/configuring-the-grub-2-boot-loader-by-using-rhel-system-roles_assembly_managing-kernel-command-line-parameters-with-uki), [GRUB mit Passwort schützen (Referenz RHEL 8, gleiches `grub2-setpassword`-Verfahren)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/managing_monitoring_and_updating_the_kernel/assembly_protecting-grub-with-a-password_managing-monitoring-and-updating-the-kernel)

### Implementation Status: partial

______________________________________________________________________

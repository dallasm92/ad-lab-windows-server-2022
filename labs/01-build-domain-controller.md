## Evidence (Click to view)

> Screenshots are linked (not embedded) to keep the lab readable.  
> Each item includes a short note on what the screenshot proves.

### 01 — VM Creation (Hyper-V)
- [01-01 — New VM name + location](../screenshots/01-vm-create/01-01_new-vm-name-location.png) — Created a new VM named **DC-1** and chose where Hyper-V stores its files.
- [01-02 — VM generation](../screenshots/01-vm-create/01-02_vm-generation.png) — Selected the VM generation (Gen 2) to use UEFI/secure boot-style firmware features.
- [01-03 — VM memory](../screenshots/01-vm-create/01-03_vm-memory.png) — Assigned startup RAM appropriate for Windows Server + AD DS lab use.
- [01-04 — Network switch](../screenshots/01-vm-create/01-04_vm-network-switch.png) — Connected the VM NIC to **vSwitch-LAB-Internal** to isolate the lab network.
- [01-05 — VHDX settings](../screenshots/01-vm-create/01-05_vhdx-settings.png) — Created the virtual disk (VHDX) used by DC-1.
- [01-06 — Attach Server ISO](../screenshots/01-vm-create/01-06_attach-server-iso.png) — Attached the Windows Server 2022 ISO to the virtual DVD drive for installation.
- [01-08 — VM wizard summary](../screenshots/01-vm-create/01-08_vm-wizard-summary.png) — Final confirmation of VM settings before creating the VM.
- [01-09 — Firmware boot order](../screenshots/01-vm-create/01-09_vm-firmware-boot-order.png) — Verified the VM can boot from DVD first for the initial OS install.

### 02 — Windows Server 2022 Installation
- [02-01 — Setup language/region](../screenshots/02-install/02-01_server-setup-language.png) — Chose installation language/keyboard settings.
- [02-02 — Install Now](../screenshots/02-install/02-02_install-now.png) — Started the Windows Server installation.
- [02-03 — Select edition (Desktop Experience)](../screenshots/02-install/02-03_select-edition-desktop-experience.png) — Selected **Standard (Desktop Experience)** for GUI tools (Server Manager, ADUC, DNS Manager).
- [02-04 — Accept license](../screenshots/02-install/02-04_accept-license.png) — Accepted license terms to proceed.
- [02-05 — Install type (Custom)](../screenshots/02-install/02-05_install-type-custom.png) — Chose **Custom** install to install fresh to the VHDX.
- [02-06 — Disk selection (unallocated)](../screenshots/02-install/02-06_disk-selection-unallocated.png) — Selected the new virtual disk as the target.
- [02-07 — Installing progress](../screenshots/02-install/02-07_installing-progress.png) — Installation in progress (files copying/configuring).
- [02-08 — Set Administrator password](../screenshots/02-install/02-08_set-admin-password-prompt.png) — Set initial local Administrator password after install.
- [02-09 — First logon (Server Manager)](../screenshots/02-install/02-09_first-logon-server-manager.png) — Confirmed successful first boot/logon and Server Manager launched.

Optional cleanup:
- [02-10 — Boot order hard drive first](../screenshots/02-install/02-10_boot-order-hard-drive-first.png) — Set boot order back to hard drive after install so it boots normally.
- [02-11 — Detach ISO (DVD none)](../screenshots/02-install/02-11_detach-iso-dvd-none.png) — Removed the ISO so the VM doesn’t try to boot into setup again.

### 03 — Lab Network Configuration (10.10.10.0/24)
- [03-01 — Local Server baseline](../screenshots/03-network/03-01_local-server-baseline.png) — Verified baseline server state before network changes.
- [03-02 — ipconfig shows APIPA](../screenshots/03-network/03-02_ipconfig-all-apipa.png) — Confirmed the VM initially had an APIPA address, showing it needed proper lab IP configuration.
- [03-03 — IPv4 static IP + DNS](../screenshots/03-network/03-03_ipv4-static-ip-dns.png) — Assigned DC-1 a static IP (**10.10.10.10/24**) and set DNS appropriately for the lab.
- [03-04 — Ping host unreachable](../screenshots/03-network/03-04_ping-10-10-10-1-unreachable.png) — Captured initial connectivity issue (useful troubleshooting evidence).
- [03-05 — Host vEthernet IP](../screenshots/03-network/03-05_host-vethernet-ip.png) — Verified the host’s **vEthernet (vSwitch-LAB-Internal)** IP is **10.10.10.1/24**.
- [03-06 — Host vEthernet IP set](../screenshots/03-network/03-06_host-vethernet-ip-set.png) — Confirmed the host interface is configured as intended.
- [03-07 — DC-1 ping host success](../screenshots/03-network/03-07_dc1-ping-host-success.png) — Verified DC-1 can reach the host over the internal lab switch.
- [03-09 — Network profile](../screenshots/03-network/03-09_network-profile.png) — Checked current Windows network profile state.
- [03-10 — Network profile private](../screenshots/03-network/03-10_network-profile-private.png) — Set network profile to **Private** to reduce firewall friction in the lab.
- [03-13 — Rename to DC-1](../screenshots/03-network/03-13_rename-computer-dc1.png) — Renamed the server to **DC-1** prior to domain promotion (best practice).
- [03-14 — hostname DC-1](../screenshots/03-network/03-14_hostname-dc1.png) — Verified the rename succeeded.

### 04 — Install AD DS Role
- [04-01 — Add Roles and Features](../screenshots/04-ad-ds/04-01_add-roles-start.png) — Started the role installation wizard in Server Manager.
- [04-02 — Role-based installation](../screenshots/04-ad-ds/04-02_role-based-installation.png) — Selected role-based install for a single server.
- [04-03 — Select DC-1](../screenshots/04-ad-ds/04-03_select-server-dc1.png) — Targeted **DC-1** as the server to receive the AD DS role.
- [04-04 — Select AD DS role](../screenshots/04-ad-ds/04-04_select-ad-ds-role.png) — Selected **Active Directory Domain Services** (and management tools).
- [04-05 — Features default](../screenshots/04-ad-ds/04-05_features-default.png) — Kept default features required for AD DS management.
- [04-06 — AD DS info](../screenshots/04-ad-ds/04-06_adds-info.png) — Reviewed role notes/requirements.
- [04-07 — Confirm install](../screenshots/04-ad-ds/04-07_confirm-install.png) — Confirmed selections before installation.
- [04-08 — Install complete](../screenshots/04-ad-ds/04-08_adds-install-complete.png) — Verified AD DS role installation completed successfully.

### 05 — Promote to Domain Controller (New Forest: lab.local)
- [05-01 — Promote this server](../screenshots/05-domain-join/05-01_promote-start.png) — Began the AD DS configuration wizard to promote DC-1.
- [05-02 — Add new forest (lab.local)](../screenshots/05-domain-join/05-02_deployment-add-new-forest-lab-local.png) — Created a new forest with root domain **lab.local**.
- [05-03 — DC options (DNS + GC)](../screenshots/05-domain-join/05-03_dc-options-dns-gc.png) — Enabled **DNS Server** and **Global Catalog**, standard for the first DC.
- [05-04 — DNS delegation warning](../screenshots/05-domain-join/05-04_dns-delegation-warning.png) — Delegation warning is expected in an isolated lab (no parent DNS zone).
- [05-05 — NetBIOS LAB](../screenshots/05-domain-join/05-05_netbios-lab.png) — Verified the NetBIOS name **LAB**.
- [05-06 — Paths default](../screenshots/05-domain-join/05-06_paths-default.png) — Used default locations for NTDS database/logs and SYSVOL (fine for a lab).
- [05-07 — Review options](../screenshots/05-domain-join/05-07_review-options.png) — Reviewed final promotion configuration before running prerequisite checks.
- [05-08 — Prerequisites passed](../screenshots/05-domain-join/05-08_prerequisites-pass.png) — Confirmed all prerequisite checks passed.
- [05-09 — Promotion installing](../screenshots/05-domain-join/05-09_promotion-installing.png) — Promotion in progress; DC reboots automatically after completion.
- [05-10 — Post-promo domain logon](../screenshots/05-domain-join/05-10_post-promo-logon-domain.png) — Verified logon context switched to the domain (LAB\\Administrator).

### 06 — Validation (DNS + AD Health)
- [05-14 — DNS zones created](../screenshots/05-domain-join/05-14_dns-zones.png) — Confirmed `lab.local` and `_msdcs.lab.local` zones exist in DNS Manager.
- [05-13 — ADUC domain tree](../screenshots/05-domain-join/05-13_aduc-domain-tree.png) — Verified the `lab.local` domain structure appears in Active Directory Users and Computers.
- [05-12 — dcdiag DNS test](../screenshots/05-domain-join/05-12_dcdiag-dns.png) — Validated DNS health using `dcdiag /test:DNS /v`.
- [05-11 — Get-ADDomain output](../screenshots/05-domain-join/05-11_get-addomain-success.png) — Confirmed AD domain configuration via PowerShell (`Get-ADDomain`).

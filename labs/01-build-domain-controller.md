# Lab 01 — Build Domain Controller (DC-1) on Windows Server 2022 (Hyper-V)

## Goal
Build a Windows Server 2022 Domain Controller in Hyper-V with **AD DS + DNS**, using an **internal lab network**.

**Outcome:** a working `lab.local` domain with verified DNS/AD health.

---

## Environment

- Host: MAIN-PC (Windows 11 Pro, Hyper-V)
- VM: DC-1 (Windows Server 2022 Standard, Desktop Experience)
- Virtual Switch: vSwitch-LAB-Internal
- Lab subnet: 10.10.10.0/24
- Host vEthernet (internal vSwitch): 10.10.10.1/24
- DC-1 IPv4: 10.10.10.10/24
- Domain: lab.local
- NetBIOS: LAB

---

## Evidence (Click to view)

> Screenshots are linked (not embedded) to keep the lab readable.  
> Each link includes a short note describing what it proves.

### 01 — VM Creation (Hyper-V)
- [01-01 — New VM name + location](../screenshots/01-vm-create/01-01_new-vm-name-location.png) — Created a new VM named **DC-1** and chose the storage location.
- [01-02 — VM generation](../screenshots/01-vm-create/01-02_vm-generation.png) — Selected the VM generation (Gen 2) for UEFI-based firmware.
- [01-03 — VM memory](../screenshots/01-vm-create/01-03_vm-memory.png) — Assigned startup RAM for Windows Server + AD DS lab work.
- [01-04 — Network switch](../screenshots/01-vm-create/01-04_vm-network-switch.png) — Connected DC-1 to **vSwitch-LAB-Internal** to isolate the lab.
- [01-05 — VHDX settings](../screenshots/01-vm-create/01-05_vhdx-settings.png) — Created the virtual hard disk used by DC-1.
- [01-06 — Attach Server ISO](../screenshots/01-vm-create/01-06_attach-server-iso.png) — Attached Windows Server 2022 ISO to the VM DVD drive.
- [01-08 — VM wizard summary](../screenshots/01-vm-create/01-08_vm-wizard-summary.png) — Final confirmation of VM settings before creation.
- [01-09 — Firmware boot order](../screenshots/01-vm-create/01-09_vm-firmware-boot-order.png) — Verified DVD boot is available for initial OS installation.

### 02 — Windows Server 2022 Installation
- [02-01 — Setup language/region](../screenshots/02-install/02-01_server-setup-language.png) — Selected language/time/keyboard settings.
- [02-02 — Install Now](../screenshots/02-install/02-02_install-now.png) — Started Windows Server installation.
- [02-03 — Select edition (Desktop Experience)](../screenshots/02-install/02-03_select-edition-desktop-experience.png) — Chose **Standard (Desktop Experience)** for GUI tools (Server Manager, ADUC, DNS).
- [02-04 — Accept license](../screenshots/02-install/02-04_accept-license.png) — Accepted license terms.
- [02-05 — Install type (Custom)](../screenshots/02-install/02-05_install-type-custom.png) — Chose **Custom** for a clean install to the VHDX.
- [02-06 — Disk selection (unallocated)](../screenshots/02-install/02-06_disk-selection-unallocated.png) — Targeted the new virtual disk.
- [02-07 — Installing progress](../screenshots/02-install/02-07_installing-progress.png) — Installation in progress.
- [02-08 — Set Administrator password](../screenshots/02-install/02-08_set-admin-password-prompt.png) — Completed initial local Administrator password setup.
- [02-09 — First logon (Server Manager)](../screenshots/02-install/02-09_first-logon-server-manager.png) — Confirmed successful first logon after installation.
- [02-10 — Boot order hard drive first](../screenshots/02-install/02-10_boot-order-hard-drive-first.png) — Switched boot order back to the hard drive after install.
- [02-11 — Detach ISO (DVD none)](../screenshots/02-install/02-11_detach-iso-dvd-none.png) — Removed the ISO to prevent booting back into setup.

### 03 — Lab Network Configuration (10.10.10.0/24)
- [03-01 — Local Server baseline](../screenshots/03-network/03-01_local-server-baseline.png) — Captured baseline state before network changes.
- [03-02 — ipconfig shows APIPA](../screenshots/03-network/03-02_ipconfig-all-apipa.png) — VM initially had APIPA, indicating no usable lab IP yet.
- [03-03 — IPv4 static IP + DNS](../screenshots/03-network/03-03_ipv4-static-ip-dns.png) — Configured DC-1 with a static IPv4 address and DNS settings.
- [03-04 — Ping host unreachable](../screenshots/03-network/03-04_ping-10-10-10-1-unreachable.png) — Captured initial connectivity issue (useful troubleshooting evidence).
- [03-05 — Host vEthernet IP](../screenshots/03-network/03-05_host-vethernet-ip.png) — Verified host vEthernet is **10.10.10.1/24**.
- [03-06 — Host vEthernet IP set](../screenshots/03-network/03-06_host-vethernet-ip-set.png) — Confirmed host internal adapter config is applied.
- [03-07 — DC-1 ping host success](../screenshots/03-network/03-07_dc1-ping-host-success.png) — Verified DC-1 can reach host over the internal switch.
- [03-09 — Network profile](../screenshots/03-network/03-09_network-profile.png) — Verified current network profile state.
- [03-10 — Network profile private](../screenshots/03-network/03-10_network-profile-private.png) — Set profile to **Private** to reduce firewall friction in lab scenarios.
- [03-13 — Rename to DC-1](../screenshots/03-network/03-13_rename-computer-dc1.png) — Renamed server to **DC-1** before domain promotion.
- [03-14 — hostname DC-1](../screenshots/03-network/03-14_hostname-dc1.png) — Confirmed rename succeeded.

### 04 — Install AD DS Role
- [04-01 — Add Roles and Features](../screenshots/04-ad-ds/04-01_add-roles-start.png) — Started the role installation wizard.
- [04-02 — Role-based installation](../screenshots/04-ad-ds/04-02_role-based-installation.png) — Selected role-based install.
- [04-03 — Select DC-1](../screenshots/04-ad-ds/04-03_select-server-dc1.png) — Targeted **DC-1** as the installation server.
- [04-04 — Select AD DS role](../screenshots/04-ad-ds/04-04_select-ad-ds-role.png) — Selected **Active Directory Domain Services** (plus management tools).
- [04-05 — Features default](../screenshots/04-ad-ds/04-05_features-default.png) — Left required features at defaults.
- [04-06 — AD DS info](../screenshots/04-ad-ds/04-06_adds-info.png) — Reviewed role notes/requirements.
- [04-07 — Confirm install](../screenshots/04-ad-ds/04-07_confirm-install.png) — Confirmed selections.
- [04-08 — Install complete](../screenshots/04-ad-ds/04-08_adds-install-complete.png) — Verified AD DS role installed successfully.

### 05 — Promote to Domain Controller (New Forest: lab.local)
- [05-01 — Promote this server](../screenshots/05-domain-join/05-01_promote-start.png) — Began promotion to domain controller.
- [05-02 — Add new forest (lab.local)](../screenshots/05-domain-join/05-02_deployment-add-new-forest-lab-local.png) — Created a new forest with root domain **lab.local**.
- [05-03 — DC options (DNS + GC)](../screenshots/05-domain-join/05-03_dc-options-dns-gc.png) — Enabled **DNS Server** and **Global Catalog** (standard for first DC).
- [05-04 — DNS delegation warning](../screenshots/05-domain-join/05-04_dns-delegation-warning.png) — Delegation warning is expected in an isolated lab (no parent zone).
- [05-05 — NetBIOS LAB](../screenshots/05-domain-join/05-05_netbios-lab.png) — Confirmed

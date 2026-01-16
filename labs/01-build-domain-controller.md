# Lab 01 — Build Domain Controller (DC-1) on Windows Server 2022 (Hyper-V)

## Goal
Build a Windows Server 2022 Domain Controller in Hyper-V with **AD DS + DNS**, using an **internal lab network**.

## Outcome
A working **lab.local** domain with verified **DNS/AD health** (dcdiag + AD PowerShell checks).

---

## Environment

- Host: MAIN-PC (Windows 11 Pro, Hyper-V)
- VM: DC-1 (Windows Server 2022 Standard, Desktop Experience)
- Virtual Switch: vSwitch-LAB-Internal (Internal)
- Lab subnet: 10.10.10.0/24
- Host vEthernet (internal vSwitch): 10.10.10.1/24
- DC-1 IPv4: 10.10.10.10/24
- Domain: lab.local
- NetBIOS: LAB

---

## Evidence (Screenshots)

Repo screenshot folders used in this lab:

- `screenshots/01-vm-create/`
- `screenshots/02-install/`
- `screenshots/03-network/`
- `screenshots/04-ad-ds/`
- `screenshots/05-domain-join/`

> If an image doesn’t render on GitHub, it’s almost always a path or filename mismatch.
> These links assume this markdown file is stored in `labs/`.

---

## Step 1 — Create the VM (Hyper-V)

Create a new VM named **DC-1**, attach the Windows Server 2022 ISO, and connect the NIC to **vSwitch-LAB-Internal**.

### Checkpoints
![New VM name/location](../screenshots/01-vm-create/01-01_new-vm-name-location.png)
![Generation](../screenshots/01-vm-create/01-02_vm-generation.png)
![Memory](../screenshots/01-vm-create/01-03_vm-memory.png)
![Network switch](../screenshots/01-vm-create/01-04_vm-network-switch.png)
![VHDX settings](../screenshots/01-vm-create/01-05_vhdx-settings.png)
![Attach Server ISO](../screenshots/01-vm-create/01-06_attach-server-iso.png)
![VM wizard summary](../screenshots/01-vm-create/01-08_vm-wizard-summary.png)
![Firmware boot order](../screenshots/01-vm-create/01-09_vm-firmware-boot-order.png)

**Why:** An internal vSwitch keeps the AD lab isolated and repeatable.

---

## Step 2 — Install Windows Server 2022

Boot from ISO and install **Windows Server 2022 Standard (Desktop Experience)**.

### Install flow
1. Windows Setup → language/region
2. Install Now
3. Choose **Desktop Experience**
4. Accept license
5. Custom install
6. Select unallocated disk
7. Install + first boot
8. Set local Administrator password
9. First logon

### Checkpoints
![Server setup language](../screenshots/02-install/02-01_server-setup-language.png)
![Install now](../screenshots/02-install/02-02_install-now.png)
![Select edition - Desktop Experience](../screenshots/02-install/02-03_select-edition-desktop-experience.png)
![Accept license](../screenshots/02-install/02-04_accept-license.png)
![Install type - Custom](../screenshots/02-install/02-05_install-type-custom.png)
![Disk selection - unallocated](../screenshots/02-install/02-06_disk-selection-unallocated.png)
![Installing progress](../screenshots/02-install/02-07_installing-progress.png)
![Set Administrator password](../screenshots/02-install/02-08_set-admin-password-prompt.png)
![First logon / Server Manager](../screenshots/02-install/02-09_first-logon-server-manager.png)

Optional cleanup after install:
![Boot order hard drive first](../screenshots/02-install/02-10_boot-order-hard-drive-first.png)
![Detach ISO (DVD none)](../screenshots/02-install/02-11_detach-iso-dvd-none.png)

---

## Step 3 — Network baseline + Static IP configuration

### 3.1 Verify baseline state
![Local Server baseline](../screenshots/03-network/03-01_local-server-baseline.png)

If the NIC shows APIPA (169.254.x.x) initially, that’s expected before you set static IP:
![ipconfig APIPA](../screenshots/03-network/03-02_ipconfig-all-apipa.png)

### 3.2 Set DC-1 static IPv4 + DNS
Configure DC-1 as:
- IP: `10.10.10.10`
- Mask: `/24`
- Gateway: (commonly left blank on internal-only labs)
- DNS: `10.10.10.10` (self, once DNS role is installed)

Checkpoint:
![IPv4 static + DNS](../screenshots/03-network/03-03_ipv4-static-ip-dns.png)

### 3.3 Verify host vEthernet for the internal switch
Host should have:
- `vEthernet (vSwitch-LAB-Internal)` = `10.10.10.1/24`

Checkpoints:
![Host vEthernet IP](../screenshots/03-network/03-05_host-vethernet-ip.png)
![Host vEthernet IP set](../screenshots/03-network/03-06_host-vethernet-ip-set.png)

### 3.4 Connectivity tests
From DC-1, ping the host `10.10.10.1`.

- If you initially got unreachable, capture it (useful troubleshooting evidence):
![Ping unreachable](../screenshots/03-network/03-04_ping-10-10-10-1-unreachable.png)

- Final expected result:
![DC-1 ping host success](../screenshots/03-network/03-07_dc1-ping-host-success.png)

Network profile checkpoints (recommended: Private for lab):
![Network profile](../screenshots/03-network/03-09_network-profile.png)
![Network profile private](../screenshots/03-network/03-10_network-profile-private.png)

---

## Step 4 — Rename the server to DC-1

Rename and reboot:

```powershell
Rename-Computer -NewName "DC-1" -Restart

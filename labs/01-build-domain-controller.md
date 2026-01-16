# Lab 01 — Build Domain Controller (DC-1) on Windows Server 2022 (Hyper-V)

## Goal
Build a Windows Server 2022 Domain Controller in Hyper-V with AD DS + DNS, using an internal lab network.  
Outcome: a working `lab.local` domain with verified DNS/AD health.

## Environment
- Host: MAIN-PC (Windows 11 Pro, Hyper-V)
- VM: DC-1 (Windows Server 2022 Standard, Desktop Experience)
- Virtual Switch: `vSwitch-LAB-Internal`
- Lab subnet: `10.10.10.0/24`
- Host vEthernet (internal vSwitch): `10.10.10.1/24`
- DC-1 IPv4: `10.10.10.10/24`
- Domain: `lab.local`
- NetBIOS: `LAB`

## Evidence (Screenshots used)
> Place screenshots in these folders (matches repo structure):
- `screenshots/02-install/` — OS installation screens
- `screenshots/03-network/` — host/vSwitch + static IP configuration
- `screenshots/04-ad-ds/` — AD DS install + promotion + validation

### AD DS Promotion Wizard + Post-promotion validation (already captured)
- `screenshots/04-ad-ds/04-01_promote-start.png`
- `screenshots/04-ad-ds/04-02_new-forest-lab-local.png`
- `screenshots/04-ad-ds/04-03_dc-options-dns-gc.png`
- `screenshots/04-ad-ds/04-04_dns-delegation-warning.png`
- `screenshots/04-ad-ds/04-05_netbios-lab.png`
- `screenshots/04-ad-ds/04-06_paths-default.png`
- `screenshots/04-ad-ds/04-07_review-options.png`
- `screenshots/04-ad-ds/04-08_prereqs-pass.png`
- `screenshots/04-ad-ds/04-09_post-promo-logon-lab-administrator.png`
- `screenshots/04-ad-ds/04-10_dns-manager-zones.png`
- `screenshots/04-ad-ds/04-11_aduc-initial-tree.png`
- `screenshots/04-ad-ds/04-12_dcdiag-dns-pass.png`
- `screenshots/04-ad-ds/04-13_get-adforest.png`
- `screenshots/04-ad-ds/04-14_get-addomain.png`

---

## Step 1 — Create the VM (Hyper-V)
1. Create a new VM named `DC-1`.
2. Attach the Windows Server 2022 ISO to the virtual DVD drive.
3. Connect the VM’s network adapter to `vSwitch-LAB-Internal`.
4. Confirm boot order includes the DVD drive first (for initial install).

**Why:** A dedicated internal vSwitch keeps the lab isolated and repeatable.

> Screenshot checkpoints (if you captured these, store in `screenshots/01-vm-create/`):
- VM settings summary (name, generation, RAM/CPU)
- Network adapter connected to `vSwitch-LAB-Internal`
- Firmware boot order showing DVD drive

---

## Step 2 — Install Windows Server 2022
1. Boot from ISO → Windows Setup.
2. Choose **Windows Server 2022 Standard (Desktop Experience)**.
3. Accept license → **Custom** install.
4. Select the unallocated disk → install.
5. Complete initial admin password prompt.

> Screenshots (place into `screenshots/02-install/` if you have them):
- Install Now screen
- Edition selection (Desktop Experience)
- Custom install + disk selection
- Installation progress

---

## Step 3 — Set computer name + reboot
Rename the server to `DC-1` and reboot:

```powershell
Rename-Computer -NewName "DC-1" -Restart

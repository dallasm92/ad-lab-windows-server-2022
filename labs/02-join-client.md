# Lab 02 - Join Windows 11 Client to Domain (`lab.local`)

## Goal
Join a Windows client VM to the `lab.local` domain and verify domain authentication, DNS behavior, and management visibility from the domain controller.

## User Impact
- Without domain join, user accounts and policies must be managed per-device.
- Access to centralized resources and identity controls is inconsistent.

## Business Impact
- Increased onboarding/offboarding effort.
- Higher risk of configuration drift and inconsistent endpoint security posture.

## Environment
- Domain Controller: `DC-1` (Windows Server 2022)
- Domain: `lab.local`
- Client VM: `WIN11-CLIENT1`
- Lab network: `10.10.10.0/24`
- DNS on client: points to DC (`10.10.10.10`)
- Client IP: `10.10.10.11`
- Host-side internal adapter: `10.10.10.1`
- Internet access: intentionally unavailable inside the isolated lab

## Evidence (Screenshots)
- `screenshots/08-client-join/`

Client-side evidence now included:
- [08-01 — Windows 11 setup getting ready](../screenshots/08-client-join/08-01_win11-setup-getting-ready.png)
- [08-02 — Windows 11 installing](../screenshots/08-client-join/08-02_win11-installing.png)
- [08-03 — Local OOBE user created for initial setup](../screenshots/08-client-join/08-03_win11-oobe-local-user.png)
- [08-04 — Client VM connected to LAB Internal switch](../screenshots/08-client-join/08-04_client-vm-network-switch.png)
- [08-05 — Initial disconnected client network state](../screenshots/08-client-join/08-05_initial-disconnected-client-state.png)
- [08-06 — Initial APIPA state before static lab network settings](../screenshots/08-client-join/08-06_initial-apipa-ipconfig.png)
- [08-07 — Static client IP and DNS set to DC-1](../screenshots/08-client-join/08-07_client-static-ip-dns.png)
- [08-08 — Client IP configuration and successful ping to DC-1](../screenshots/08-client-join/08-08_client-ipconfig-ping-dc.png)
- [08-09 — `lab.local` and `dc-1.lab.local` resolve through DC DNS](../screenshots/08-client-join/08-09_client-nslookup-lab-local.png)
- [08-10 — Client renamed to `WIN11-CLIENT1`](../screenshots/08-client-join/08-10_rename-client-win11-client1.png)
- [08-11 — Renamed client retains static lab IP and DNS](../screenshots/08-client-join/08-11_renamed-client-ipconfig.png)
- [08-12 — Client validates DC connectivity and DNS before join](../screenshots/08-client-join/08-12_client-dc-dns-validation.png)
- [08-13 — `Add-Computer` domain join command](../screenshots/08-client-join/08-13_add-computer-domain-join.png)
- [08-14 — Domain join credential prompt](../screenshots/08-client-join/08-14_domain-join-credentials.png)
- [08-15 — Domain join restart required](../screenshots/08-client-join/08-15_domain-join-restart-required.png)
- [08-16 — Domain sign-in prompt after join](../screenshots/08-client-join/08-16_domain-signin-lab-administrator.png)
- [08-17 — Post-join domain logon variables on client](../screenshots/08-client-join/08-17_post-join-domain-logon-vars.png)
- [08-18 — `Get-ADComputer` finds `WIN11-CLIENT1` on DC-1](../screenshots/08-client-join/08-18_get-adcomputer-client-object.png)
- [08-19 — Client resolves and pings `dc-1.lab.local`](../screenshots/08-client-join/08-19_client-fqdn-dns-ping-validation.png)
- [08-20 — `systeminfo` shows domain `lab.local`](../screenshots/08-client-join/08-20_systeminfo-domain-lab-local.png)
- [08-21 — ADUC shows the joined client computer object](../screenshots/08-client-join/08-21_aduc-client-computer-object.png)
- [08-22 — Helpdesk user signed in on `WIN11-CLIENT1`](../screenshots/08-client-join/08-22_helpdesk-user-client-session.png)
- [08-23 — RSAT optional features unavailable to the helpdesk user](../screenshots/08-client-join/08-23_rsat-optional-features-unavailable.png)
- [08-24 — Helpdesk session validates DC/host connectivity and DNS](../screenshots/08-client-join/08-24_helpdesk-client-ping-dns-validation.png)
- [08-25 — Helpdesk session final `ipconfig /all`](../screenshots/08-client-join/08-25_helpdesk-client-ipconfig-final.png)
- [08-26 — `LAB\GG_Helpdesk` appears in the helpdesk user's token](../screenshots/08-client-join/08-26_helpdesk-client-whoami-groups.png)

## Steps

### 1) Confirm Client Network and DNS
On client PowerShell:

```powershell
ipconfig /all
ping 10.10.10.10
nslookup lab.local
```

Expected:
- Client has valid lab subnet IP
- DC is reachable by ping
- DNS resolves `lab.local` using DC as resolver
- No internet route is required for domain join in this isolated lab

### 2) Join the Domain
Client path:
- Settings -> System -> About -> Domain or workgroup
- Join domain: `lab.local`
- Provide domain admin credentials
- Reboot client

Alternative PowerShell method:

```powershell
Add-Computer -DomainName lab.local -Credential LAB\Administrator -Restart
```

### 3) Verify Domain Logon
After reboot, sign in as a domain user, example:
- `LAB\dmorison`

Run checks:

```powershell
whoami
echo $env:USERDOMAIN
echo $env:LOGONSERVER
```

Expected:
- User context shows `LAB\...`
- Domain variable is `LAB`
- Logon server points to `DC-1`

In this lab, I validated sign-in with the delegated helpdesk account:

```powershell
whoami
whoami /groups
```

Expected:
- `whoami` shows the signed-in domain account.
- `whoami /groups` includes `LAB\GG_Helpdesk`.

### 4) Verify Visibility from DC
On DC-1:
- Open Active Directory Users and Computers
- Confirm client computer object appears under `Computers` (or target OU)

Optional command:

```powershell
Get-ADComputer -Identity WIN11-CLIENT1
```

## Validation Checklist
- Domain join completed and required a restart
- Domain user logon worked on `WIN11-CLIENT1`
- Client resolved `lab.local` through DC DNS
- Client reached the domain controller and host-side internal adapter
- Client computer object was visible from AD tooling on `DC-1`
- `LAB\GG_Helpdesk` appeared in the helpdesk user's client logon token

## Issues and Fixes
Common issue: domain join fails with "domain not found"
- Root cause: client DNS not pointing to DC
- Fix: set client DNS to DC IP and retry join

Common issue: authentication fails during join
- Root cause: wrong credential format or time skew
- Fix: use `LAB\Administrator` and verify time sync between DC/client

Issue encountered: account looked like it had a password problem, but the real problem was username format.
- Fix: use the correct domain username format, such as `LAB\Helpdesk.User` or `Helpdesk.User` at the domain sign-in prompt.

Issue encountered: RSAT / ADUC was unavailable on the Windows 11 client.
- Root cause: isolated lab had no internet path for downloading optional tools.
- Fix: use the built-in AD management tools already present on `DC-1`.

## Prevention / Notes
- Always set client DNS to domain controller before domain join.
- Keep VM snapshots before major identity/network changes.
- Use a standard naming convention for client objects (e.g., `WIN11-CLIENT1`).
- In an isolated lab, plan for no internet and validate with built-in Windows tools first.

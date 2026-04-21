# Lab 04 - Helpdesk Delegation Troubleshooting

## Goal

Validate a delegated helpdesk-style account in the `lab.local` domain and document the difference between:

- signing in to a domain-joined Windows 11 client
- administering Active Directory from the domain controller
- having group membership
- having local logon rights
- having enough elevation to run administrative MMC tools

## Environment

- Domain Controller: `DC-1` (Windows Server 2022)
- Client VM: `WIN11-CLIENT1` (Windows 11)
- Domain: `lab.local`
- Lab network: `10.10.10.0/24`
- DC IP: `10.10.10.10`
- Client IP: `10.10.10.11`
- Host-side internal adapter: `10.10.10.1`
- Internet access: intentionally unavailable in the isolated lab

## Evidence

Helpdesk object and group setup:
- [09-01 — Helpdesk user present in ADUC](../screenshots/09-helpdesk-delegation/09-01_aduc-helpdesk-user-present.png)
- [09-02 — Helpdesk user is member of `Domain Users` and `GG_Helpdesk`](../screenshots/09-helpdesk-delegation/09-02_helpdesk-user-member-of-gg-helpdesk.png)
- [09-03 — Lab Users OU shows helpdesk and test users](../screenshots/09-helpdesk-delegation/09-03_aduc-lab-users-list.png)

Client-side validation:
- [08-22 — Helpdesk user signed in on `WIN11-CLIENT1`](../screenshots/08-client-join/08-22_helpdesk-user-client-session.png)
- [08-24 — Helpdesk client session validates DC/host connectivity and DNS](../screenshots/08-client-join/08-24_helpdesk-client-ping-dns-validation.png)
- [08-25 — Helpdesk client session final `ipconfig /all`](../screenshots/08-client-join/08-25_helpdesk-client-ipconfig-final.png)
- [08-26 — `LAB\GG_Helpdesk` appears in the helpdesk user's token](../screenshots/08-client-join/08-26_helpdesk-client-whoami-groups.png)

Troubleshooting and policy evidence:
- [08-23 — RSAT optional features unavailable on the client](../screenshots/08-client-join/08-23_rsat-optional-features-unavailable.png)
- [09-04 — Delegation wizard start](../screenshots/09-helpdesk-delegation/09-04_delegation-wizard-start.png)
- [09-05 — Select `GG_Helpdesk` for delegation](../screenshots/09-helpdesk-delegation/09-05_select-gg-helpdesk-for-delegation.png)
- [09-06 — `GG_Helpdesk` selected for delegation](../screenshots/09-helpdesk-delegation/09-06_gg-helpdesk-selected-for-delegation.png)
- [09-07 — Common delegated tasks selected](../screenshots/09-helpdesk-delegation/09-07_delegation-common-tasks.png)
- [09-08 — Delegation wizard completed](../screenshots/09-helpdesk-delegation/09-08_delegation-complete.png)
- [09-09 — Helpdesk group membership after delegation work](../screenshots/09-helpdesk-delegation/09-09_helpdesk-membership-after-delegation.png)
- [09-10 — `dsa.msc` runas attempt blocked by domain-controller logon type](../screenshots/09-helpdesk-delegation/09-10_dsa-msc-logon-type-not-granted.png)
- [09-11 — Local Security Policy `Allow log on locally`](../screenshots/09-helpdesk-delegation/09-11_local-security-policy-allow-logon.png)
- [09-12 — Default Domain Controllers Policy in GPMC](../screenshots/09-helpdesk-delegation/09-12_default-domain-controllers-policy.png)
- [09-13 — GPO `Allow log on locally` reviewed/edited](../screenshots/09-helpdesk-delegation/09-13_gpo-allow-logon-locally.png)
- [09-14 — GPO `Deny log on locally` checked for explicit deny](../screenshots/09-helpdesk-delegation/09-14_gpo-deny-logon-locally-empty.png)
- [09-15 — `gpupdate /force` completed successfully](../screenshots/09-helpdesk-delegation/09-15_gpupdate-force-success.png)
- [09-16 — `runas` still blocked by elevation requirement](../screenshots/09-helpdesk-delegation/09-16_runas-dsa-msc-elevation-required.png)
- [09-17 — MMC UAC prompt confirms elevation boundary](../screenshots/09-helpdesk-delegation/09-17_mmc-uac-elevation-required.png)
- [09-18 — DC confirms internal connectivity and no internet path](../screenshots/09-helpdesk-delegation/09-18_dc-internal-ping-no-internet.png)

## Steps Performed

### 1) Created Helpdesk Identity Objects

On `DC-1`, I created:

- user: `Helpdesk.User`
- security group: `GG_Helpdesk`

I verified the helpdesk user was a member of:

- `Domain Users`
- `GG_Helpdesk`

### 2) Validated Client Sign-In and Group Token

On `WIN11-CLIENT1`, I signed in as the helpdesk user and ran:

```powershell
hostname
ipconfig /all
whoami
whoami /groups
ping 10.10.10.10
ping 10.10.10.1
nslookup lab.local
```

Validation points:

- client hostname was `WIN11-CLIENT1`
- domain account context was `LAB\Helpdesk.User`
- DNS server pointed to `DC-1`
- `lab.local` resolved through the domain controller
- the client could reach the domain controller and host-side internal adapter
- `LAB\GG_Helpdesk` appeared in the signed-in user's token

### 3) Identified Missing RSAT / ADUC on the Client

The Windows 11 client did not have RSAT / Active Directory Users and Computers installed.

Because the lab had no internet access, I could not download optional features during the session. The practical fix was to use the tools already available on `DC-1` instead of trying to administer AD from the client.

### 4) Tested Domain Controller Logon Rights

When attempting to use the helpdesk account on `DC-1`, I hit a logon-right failure:

- the username/password was not the root issue
- the account lacked the required interactive logon rights on the domain controller

I reviewed:

- Local Security Policy
- User Rights Assignment
- `Allow log on locally`
- `Deny log on locally`

### 5) Updated Domain Controller Logon Policy

In Group Policy Management, I reviewed the Default Domain Controllers Policy:

```text
Computer Configuration
Policies
Windows Settings
Security Settings
Local Policies
User Rights Assignment
```

I checked `Allow log on locally` and confirmed `Deny log on locally` was not explicitly blocking the account.

After policy edits, I forced refresh:

```powershell
gpupdate /force
```

### 6) Confirmed Elevation Boundary

After logon-right troubleshooting, I still hit UAC / elevation boundaries when trying to run MMC-based AD tools under the delegated helpdesk account.

That was an important distinction:

- authentication worked
- group membership worked
- client sign-in worked
- logon-right policy could be adjusted
- the account still was not a full administrator on the domain controller
- ADUC/MMC administration may require elevation depending on how the task is launched

## Key Lessons

- A failed sign-in is not always a bad password. Username format matters.
- Client sign-in and domain-controller sign-in are different workflows with different rights.
- RSAT is convenient, but not required if the domain controller already has AD tools.
- In an isolated lab, assume you cannot download missing features during troubleshooting.
- Group membership proves identity assignment, but it does not automatically grant local logon or elevation rights.
- The cleanest workflow for this lab is:
  - use `DC-1` as Administrator for server-side AD administration
  - use `WIN11-CLIENT1` as `Helpdesk.User` for client sign-in, DNS, connectivity, and group-token validation

## Final Outcome

The lab validated a working domain controller, a domain-joined Windows 11 client, a helpdesk user, and a helpdesk security group. It also documented realistic troubleshooting around username format, missing RSAT, no internet access, domain-controller logon rights, Group Policy refresh, and UAC/elevation limits.

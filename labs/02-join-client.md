# Lab 02 - Join Windows Client to Domain (`lab.local`)

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

## Evidence (Screenshots)
- `screenshots/05-domain-join/`

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

### 4) Verify Visibility from DC
On DC-1:
- Open Active Directory Users and Computers
- Confirm client computer object appears under `Computers` (or target OU)

Optional command:

```powershell
Get-ADComputer -Identity WIN11-CLIENT1
```

## Validation Checklist
- Domain join completed without error
- Domain user logon successful on client
- Client resolves domain via DC DNS
- Client computer object present in AD

## Issues and Fixes
Common issue: domain join fails with "domain not found"
- Root cause: client DNS not pointing to DC
- Fix: set client DNS to DC IP and retry join

Common issue: authentication fails during join
- Root cause: wrong credential format or time skew
- Fix: use `LAB\Administrator` and verify time sync between DC/client

## Prevention / Notes
- Always set client DNS to domain controller before domain join.
- Keep VM snapshots before major identity/network changes.
- Use a standard naming convention for client objects (e.g., `WIN11-CLIENT1`).

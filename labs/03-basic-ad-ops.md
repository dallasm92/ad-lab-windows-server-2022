# Lab 03 - Basic AD Operations (OUs, Users, Groups, Basic GPO)

## Goal
Create a basic organizational model in Active Directory, provision users/groups, and apply a simple GPO to demonstrate policy management workflow.

## User Impact
- Users need consistent access and predictable endpoint behavior.
- Poorly structured AD objects cause confusion and support friction.

## Business Impact
- Inconsistent policy/application across endpoints increases support overhead.
- Weak identity/group structure slows onboarding and access control changes.

## Evidence (Screenshots)
- `screenshots/06-users-groups/`
- `screenshots/07-gpos/`

## Steps

### 1) Create OU Structure
Example OU design:
- `Lab Users`
- `Lab Computers`
- `Lab Groups`

In ADUC:
- Right-click domain -> New -> Organizational Unit

### 2) Create Users and Groups
Example users:
- `dmorison`
- `testuser1`

Example groups:
- `GG-Lab-FileShare-Read`
- `GG-Lab-FileShare-Modify`

Group guidance:
- Use Global Security groups for user access assignment in a single domain lab.

### 3) Assign Group Membership and Validate Access Intent
Add users to groups by job/role intent.

Verification in PowerShell:

```powershell
Get-ADUser dmorison -Properties MemberOf
Get-ADGroupMember "GG-Lab-FileShare-Modify"
```

### 4) Create and Link a Basic GPO
Example baseline GPO:
- Name: `Lab-Workstation-Baseline`
- Link target: `Lab Computers` OU

Example settings (starter level):
- Disable Control Panel access (test policy)
- Set a desktop wallpaper policy (visual verification)

### 5) Validate Policy Application on Client
On domain-joined client:

```powershell
gpupdate /force
gpresult /r
```

Expected:
- `Lab-Workstation-Baseline` appears under applied computer policies

## Validation Checklist
- OU structure created and visible
- Users and groups created with intended naming
- Group memberships match access design
- GPO linked to correct OU
- Policy applies successfully on target client

## Issues and Fixes
Common issue: GPO not applying
- Root cause: linked to wrong OU or security filtering mismatch
- Fix: verify object location, link status, and policy scope

Common issue: user/group confusion
- Root cause: inconsistent naming conventions
- Fix: adopt standard prefixes (`GG-`, `DL-`) and document intent in description fields

## Prevention / Notes
- Keep OU/group naming conventions simple and consistent.
- Validate each AD change with command output, not UI only.
- Use a "test user + test client" pair before broad policy rollout.
